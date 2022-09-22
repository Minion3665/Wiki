# A journey in C++: Adding nix installables to scope

## Our notes from our last try at this

## Previously we had

```diff
-            auto e = state->parseExprFromString(*expr, absPath("."));
-            state->eval(e, *vFile);
```

This parsed the expression and evaluated it

## To evaluate, nix repl does

```cpp
Expr * e = state->parseExprFromString(std::move(s), curDir, staticEnv);
e->eval(*state, *env, v);
```

This has the advantage of taking an extra parameter, `env` which we may be able
to use to store extra attributes, indeed when adding vars to scope nix repl does
this

````cpp
void NixRepl::addVarToScope(const Symbol name, Value & v)
{
    if (displ >= envSize)
        throw Error("environment full; cannot add more variables");
    if (auto oldVar = staticEnv->find(name); oldVar != staticEnv->vars.end())
        staticEnv->vars.erase(oldVar);
    staticEnv->vars.emplace_back(name, displ);
    staticEnv->sort();
    env->values[displ++] = &v;
    varNames.emplace(state->symbols[name]);
}```

It is unclear at present what `staticEnv` is used for. My guess is that it's a
copy of the environment (that isn't changed by evaluating nix expressions) and
we therefore don't need to worry about it, however this may be purely wishful
thinking...

## To evaluate, our intermediary patch does
```cpp
Strings e = {};

for (auto & s : explicitInstallables.value_or(std::vector<std::string>())) {
    std::exception_ptr ex;

    if (s.find('/') != std::string::npos) {
        try {
            result.push_back(std::make_shared<InstallableStorePath>(store, store->followLinksToStorePath(s)));
            continue;
        } catch (BadStorePath &) {
        } catch (...) {
            if (!ex)
                ex = std::current_exception();
        }
    }

    try {
        auto [flakeRef, fragment, outputsSpec] = parseFlakeRefWithFragmentAndOutputsSpec(s, absPath("."));
        auto installableFlake = InstallableFlake(
                this,
                getEvalState(),
                std::move(flakeRef),
                fragment,
                outputsSpec,
                getDefaultFlakeAttrPaths(),
                getDefaultFlakeAttrPathPrefixes(),
                lockFlags);
        auto lockedFlake = installableFlake.getLockedFlake()->flake.lockedRef;

        auto defaultPathPrefixes = getDefaultFlakeAttrPathPrefixes();
        defaultPathPrefixes.push_back("");

        for (auto & path : defaultPathPrefixes) {
            if (path.length()) {
                path.pop_back();
                path = "." + path;
            }

            e.push_back(str(boost::format("with (builtins.getFlake \"%1%\").outputs%2%%3% or {}") % lockedFlake.to_string() % path % (fragment != "" ? "." + fragment : "")));
        }
        continue;
    } catch (...) {
        ex = std::current_exception();
    }

    std::rethrow_exception(ex);
}

e.push_back(*expr);
auto parsed = state->parseExprFromString(concatStringsSep(";", e), absPath("."));
state->eval(parsed, *vFile);

if (!ss.empty()) {
    auto [prefix, outputsSpec] = parseOutputsSpec(".");
    result.push_back(
        std::make_shared<InstallableAttrPath>(
            state, *this, vFile,
            prefix == "." ? "" : prefix,
            outputsSpec));
}
return result;
````

Although length is not an issue, this still is problematic as it uses `with` and
prepends to the original expression. This means that when you type a bad
expression, you'll get something like this

```sh
$ nix shell nixpkgs#hello --expr "fail"
"
error: undefined variable 'fail'

       at «string»:1:747:

            1| with (builtins.getFlake "path:/nix/store/46v5hkwaa7aadrdn3gadlcx
            8ckrv3brj-source?lastModified=1663491030&narHash=sha256-MVsfBhE9US5
            DvLtBAaTRjwYdv1tLO8xjahM8qLXTgTo=&rev=767542707d394ff15ac1981e903e0
            05ba69528b5").outputs.packages.x86_64-linux.hello or {};with
            (builtins.getFlake "path:/nix/store/46v5hkwaa7aadrdn3gadlcx8ckrv3br
            j-source?lastModified=1663491030&narHash=sha256-MVsfBhE9US5DvLtBAaT
            RjwYdv1tLO8xjahM8qLXTgTo=&rev=767542707d394ff15ac1981e903e005ba6952
            8b5").outputs.legacyPackages.x86_64-linux.hello or {};with (builtin
            s.getFlake "path:/nix/store/46v5hkwaa7aadrdn3gadlcx8ckrv3brj-source
            ?lastModified=1663491030&narHash=sha256-MVsfBhE9US5DvLtBAaTRjwYdv1t
            LO8xjahM8qLXTgTo=&rev=767542707d394ff15ac1981e903e005ba69528b5").
            outputs.hello or {};fail
             |






                                                             ^
```

(_some line breaks were added for better wrapping_)

As you can see, this leads to a very broken error message, which both contains
_far_ too many lines afterwards in the hope of you having an extremely wide
terminal, as well as including the full locked paths to every installable you
specified 3 times over. Let's not.

A far better solution would be to do something similar to the environment method
that nix repl uses. Let's see about that then.

## Part 1: Adding an environment

My hope is that this'll be fairly simple, as it ought to be pretty much just a
copy of what we have in Nix repl. As we're only adding an environment, we also
won't have to evaluate any expressions or anything beforehand.

I'm going to revert the hunk of the installables.cc file that handles evaluating
this to before I started working on this patch, as I think this will provide the
best starting point without losing too much. I will, of course, review my diff
after writing the full patch in order to avoid issues with old parts of the
patch staying in when they are no longer needed.

```vim
:Git reset upstream/master %
:GitGutterUndoHunk
" 62 fewer lines
```

Now, I suspect we can use `e` rather than `state` to evaluate, passing in the
state. The question is if we need to: let's see if state can take an env
parameter...

State is got by calling `getEvalState()`

```cpp
ref<EvalState> EvalCommand::getEvalState();
```

And `getEvalState` returns a reference to `EvalState`

Let's take a look at EvalState and see how we can run `eval` on it

```cpp
void EvalState::eval(Expr * e, Value & v)
{
    e->eval(*this, baseEnv, v);
}
```

As is typical, there's both good and bad news. The bad news is that this is the
only instance of eval. The good news is that this is very obviously showing us
that we can evaluate directly at the expression. Let's do that:

```cpp
auto e = state->parseExprFromString(*expr, absPath("."));
e->eval(state, env, *vFile);
```

And so now the only question is "what do we put in env"? Currently we use
`baseEnv`, so to avoid breaking anything that should probably be the minimum set
of values in the environment-- of course I don't actually know what `baseEnv` is
so I can't be certain that it even has any values in it. Let's see how `NixRepl`
initializes its `env` to give us a better idea of what to do

```cpp
void NixRepl::initEnv()
{
    env = &state->allocEnv(envSize);
    env->up = &state->baseEnv;
    displ = 0;
    staticEnv->vars.clear();

    varNames.clear();
    for (auto & i : state->staticBaseEnv->vars)
        varNames.emplace(state->symbols[i.first]);
}
```

This looks like it. Happily it seems that it probably does get the baseEnv.
Let's just copy this then. Of course, we do need to have `varNames` and
`staticEnv` for this to run, so let's see if we can find out what they are

`varNames` is a `StringSet`, and by its name it probably holds the names of our
variables. I think we can probably safely ignore it as I imagine it's used for
completion in the repl, but not much WRT evaluating expressions

We assumed earlier that StaticEnv wasn't used, in evaluation so I'm going to
continue under that assumption. We'll come back here later if it turns out to be
wrong

`displ` seems to store the size of the environment currently (hence why it is
initialized to 0). If we are careful, we can size our environment to always have
just enough space in it, so for now I'll remove that variable

Lastly, `envSize` presumably stores the size that we want the environment to be.
Given this, we can set it to 0 (as we know that it must be 0 when `displ` is set
to 0 and we can allocate it based on how much stuff we want to store later)

And with that, we have a much simplified environment initialization sequence:

```cpp
auto env = &state->allocEnv(0);
env->up = &state->baseEnv;
auto e = state->parseExprFromString(*expr, absPath("."));
e->eval(state, env, *vFile);
```

Let's build and check that it works, and if it does we'll move on to the next
part.
