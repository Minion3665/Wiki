# A journey in C++: Adding nix installables to scope

## Our notes from our last try at this

### Adding flakes to the expression scope in nix shell and nix run

This change was inspired by
[this comment on github](https://github.com/NixOS/nix/issues/5567#issuecomment-991130214)
which suggests an (I believe elegant) solution to being able to run expressions
to drop to nix shells more easily

For starters, the code that we'll need to change is in
[https://github.com/NixOS/nix](https://github.com/NixOS/nix), so I cloned that
repo into `/home/minion/Code/c-cpp/nix` (nix is written largely in cpp) The
commands all appear to be in the `src` folder, layed out in folders by base
command name. ![image.png](files/image.png) As I'm using the new command line,
commands like `nix-env` and `nix-build` have all been replaced with the single
`nix` command; so I'll look in there first. ![image.png](files/image_p.png)
Inside nix we appear to have 2 things for most subcommands- a cpp file and a
markdown file. I suspect the markdown files are the documentation- let's check
that. ![image.png](files/image_l.png) Absolutely! Alright, so presumably the
`.cc` file that goes along with these is the source for the command. Let's take
a look at `shell.cc`, since as that's the command we want to modify. But wait—
hold up. There's a file called `shell.md` but we don't have a `shell.cc`...
![image.png](files/image_1.png) This means that the code from shell must be
somewhere else. Let's hop into another command and see if we can see anything
about the command structure from there. Perhaps there's something I can grep to
find out where shell ended up. I'll choose `eval.cc`, as I suspect it to be one
of the simpler commands. Alright, so eval is 129 lines which just run a nix
expression. It looks like commands are structs that implement
InstallableCommand. They have a run method, which I assume is what is executed
when the command is run, as well as a decription and a 'doc', which just gets
the md file. Importantly, they also have a command registration at the bottom.
I'm going to grep for that. ![image.png](files/image_e.png)
![image.png](files/image_c.png) Awesome! It looks like the shell command is
registered in `run.cc`, which makes some sense as `nix run` and `nix shell` are
very similar commands. I'm going to open it up and have a read. The file is 272
lines long, and contains both run and shell. It doesn't contain any other
commands. As expected, both of them run pretty similarly, doing some processing
beforehand and then calling a `runProgramInStore` function.
![image.png](files/image_o.png) From the issue, the way to do this currently is
through the `--expr` argument.

```
nix shell --impure --expr 'with builtins.getFlake "nixpkgs"; with legacyPackages.${builtins.currentPlatform}; python3.withPackages (ps: with ps; [ numpy scipy ])'

```

So let's take a look and see if we can figure out where expr is processed in the
shell and run commands. ![image.png](files/image_m.png) A simple search for
`expr` yields no results. That means that the code that runs expr may be in a
different file. I suspect that whatever sets up the environment for our shell
(i.e. by installing the necessary packages) also runs the expression, as that
seems like the common-sense place to put it. `runProgramInStore` takes a store
ref and a program to run, so I think that it's too late in the process (i.e.
after the store is already set up). That means that it's probably called from
the run method of the shell and run commands

```
    void run(ref<Store> store) override
    {
        auto outPaths = Installable::toStorePaths(getEvalStore(), store, Realise::Outputs, OperateOn::Output, installables);

        auto accessor = store->getFSAccessor();

        std::unordered_set<StorePath> done;
        std::queue<StorePath> todo;
        for (auto & path : outPaths) todo.push(path);

        setEnviron();

        auto unixPath = tokenizeString<Strings>(getEnv("PATH").value_or(""), ":");

        while (!todo.empty()) {
            auto path = todo.front();
            todo.pop();
            if (!done.insert(path).second) continue;

            if (true)
                unixPath.push_front(store->printStorePath(path) + "/bin");

            auto propPath = store->printStorePath(path) + "/nix-support/propagated-user-env-packages";
            if (accessor->stat(propPath).type == FSAccessor::tRegular) {
                for (auto & p : tokenizeString<Paths>(readFile(propPath)))
                    todo.push(store->parseStorePath(p));
            }
        }

        setenv("PATH", concatStringsSep(":", unixPath).c_str(), 1);

        Strings args;
        for (auto & arg : command) args.push_back(arg);

        runProgramInStore(store, *command.begin(), args);
    }
```

This is the run method of the shell command. The last thing in this function is
the `runProgramInStore` call. It also takes in a store reference- perhaps the
store reference actually just refers to the nix store where all of your outputs
are? It's conventionally called `/nix/store` but it can be called anything so
that's probably what that is for. Interestingly, though, it looks like I am
right about my assumption for where the program assembles our environment. We're
setting up the `$PATH` variable here, which tells our shell where to look for
executables when we run a command.

```
        auto unixPath = tokenizeString<Strings>(getEnv("PATH").value_or(""), ":");

        while (!todo.empty()) {
            auto path = todo.front();
            todo.pop();
            if (!done.insert(path).second) continue;

            if (true)
                unixPath.push_front(store->printStorePath(path) + "/bin");

            auto propPath = store->printStorePath(path) + "/nix-support/propagated-user-env-packages";
            if (accessor->stat(propPath).type == FSAccessor::tRegular) {
                for (auto & p : tokenizeString<Paths>(readFile(propPath)))
                    todo.push(store->parseStorePath(p));
            }
        }

        setenv("PATH", concatStringsSep(":", unixPath).c_str(), 1);

```

Specifically this looks like the important part. Let's read a little bit so we
can understand what's going on.

```
auto unixPath = tokenizeString<Strings>(getEnv("PATH").value_or(""), ":");
```

[Tokenizing just means splitting up a string](https://www.geeksforgeeks.org/tokenizing-a-string-cpp/),
here we have our path variable which is colon delimited. `tokenizeString`
presumably splits it up into an array of strings.

```
while (!todo.empty()) {
    // ...
}
```

There's a variable named `todo`. Let's see if we can figure out where it comes from.

```
std::queue<StorePath> todo;
for (auto & path : outPaths) todo.push(path);

```

Alright! It looks like `todo` is simply the list of store paths that we want to
have access to in our shell... that means that the while loop probably isn't the
important bit, as although it adds the paths to our shell I suspect it doesn't
actually go and install the packages (or more importantly go and evalulate the
expressions). So that means that the important bit is probably in here...

```
        auto outPaths = Installable::toStorePaths(getEvalStore(), store, Realise::Outputs, OperateOn::Output, installables);

        auto accessor = store->getFSAccessor();

        std::unordered_set<StorePath> done;
        std::queue<StorePath> todo;
        for (auto & path : outPaths) todo.push(path);

        setEnviron();

        auto unixPath = tokenizeString<Strings>(getEnv("PATH").value_or(""), ":");

```

So let's look at this line-by-line instead...

```
auto outPaths = Installable::toStorePaths(getEvalStore(), store, Realise::Outputs, OperateOn::Output, installables);
```

This line sets a variable 'outPaths' (quite a promising name, as it sounds like
we're looking at the store paths for stuff here?) to the output of a function
`toStorePaths` Nevertheless, toStorePaths and getEvalStore appear to be stored
elsewhere, so we're going to have to find where they're defined. Time for some
more grep. First up, toStorePaths: It looks like it's defined in
`installables.cc`: let's go there.

```
StorePathSet Installable::toStorePaths(
    ref<Store> evalStore,
    ref<Store> store,
    Realise mode, OperateOn operateOn,
    const std::vector<std::shared_ptr<Installable>> & installables)
{
    StorePathSet outPaths;
    for (auto & path : toBuiltPaths(evalStore, store, mode, operateOn, installables)) {
        auto thisOutPaths = path.outPaths();
        outPaths.insert(thisOutPaths.begin(), thisOutPaths.end());
    }
    return outPaths;
}

```

Alright, so, this function appears to return a set of store paths, it uses a
["range based for loop"](https://en.cppreference.com/w/cpp/language/range-for)
in order to iterate through the outputs of a function called `toBuiltPaths`, and
then [inserts the `outPaths` from that into its set of store paths](https://www.geeksforgeeks.org/set-insert-function-in-c-stl/#:~:text=The%20set%3A%3Ainsert%20is,set%20to%20a%20different%20set.&text=Parameters%3A%20The%20function%20accepts%20a,inserted%20in%20the%20set%20container.).
Let's go look at `toBuiltPaths` and see if we can get an idea of what's going on
there too

```
BuiltPaths Installable::toBuiltPaths(
    ref<Store> evalStore,
    ref<Store> store,
    Realise mode,
    OperateOn operateOn,
    const std::vector<std::shared_ptr<Installable>> & installables)
{
    if (operateOn == OperateOn::Output)
        return Installable::build(evalStore, store, mode, installables);
    else {
        if (mode == Realise::Nothing)
            settings.readOnlyMode = true;

        BuiltPaths res;
        for (auto & drvPath : Installable::toDerivations(store, installables, true))
            res.push_back(BuiltPath::Opaque{drvPath});
        return res;
    }
}

```

I'm going to be honest here: I'm not sure what this function does. It seems to
be building derivations and then returning their paths, but I no longer feel
that persuing it would be useful without first knowing some other stuff— let's
actually make a checklist of questions I have

- [ ] Where are arguments passed to commands validated & processed? How would a
- run function get at them? [ ] Is a StorePathSet actually a `set<StorePath>`?
- If not, what's the difference? If yes, why is it defined like this? [ ] What's
- the difference between an `evalStore` and a `store`?  
   And, of course, it'd be useful to look at the rest of the arguments that
  are passed in to the toStorePaths function: perhaps there's something
  that makes it easier

```
auto outPaths = Installable::toStorePaths(getEvalStore(), store, Realise::Outputs, OperateOn::Output, installables);
```

Taking a look, there definitely is. Realize is _always_ Outputs, OperateOn is
_always_ Output (I'm assuming that both of these are enums or similar). That
actually cuts down the toBuiltPaths down a _lot_.

```
// Not actual Nix Code, purely for illustration
BuiltPaths Installable::toBuiltPaths(
    ref<Store> evalStore,
    ref<Store> store,
    const std::vector<std::shared_ptr<Installable>> & installables)
{
    return Installable::build(evalStore, store, Realise::Outputs, installables);
}
```

However, right now that's still an aside. I at the very least want to figure out
where arguments are registered and processed first, as that's something which I
probably should have done right at the start. I'm still going to add it to the
list though:

- [ ] What does toBuiltPaths actually _do_?  
       Alright, first thing is to figure out what happens when I register a
      command. Ripgrepping registerCommand leads us to command.hh as a
      definition point:

```
template<class T>
static RegisterCommand registerCommand(const std::string & name)
{
    return RegisterCommand({name}, [](){ return make_ref<T>(); });
}

```

It returns something of type RegisterCommand, defined right above

```
struct RegisterCommand
{
    typedef std::map<std::vector<std::string>, std::function<ref<Command>()>> Commands;
    static Commands * commands;

    RegisterCommand(std::vector<std::string> && name,
        std::function<ref<Command>()> command)
    {
        if (!commands) commands = new Commands;
        commands->emplace(name, command);
    }

    static nix::Commands getCommandsFor(const std::vector<std::string> & prefix);
};
```

Observing where this was used before, it seems that almost nothing outside of
the nix folder uses it, or rather this registerCommand function is _only_ used
to register commands under `nix` (i.e. new-style commands)  
![image.png](files/image_x.png)  
[Emplace just sets the command under name to the value](https://en.cppreference.com/w/cpp/container/map/emplace),
think of it as the equivalent to something like

```
def emplace(dictionary, key, value):
 dictionary[key] = value

```

Looking at run.cc again, it seems like there's a function named addFlag called
which I assume is adding a flag to the command

```
    CmdShell()
    {
        addFlag({
            .longName = "command",
            .shortName = 'c',
            .description = "Command and arguments to be executed, defaulting to `$SHELL`",
            .labels = {"command", "args"},
            .handler = {[&](std::vector<std::string> ss) {
                if (ss.empty()) throw UsageError("--command requires at least one argument");
                command = ss;
            }}
        });
    }

```

Let's see if we can figure out where the expr flag is added; assuming that the
same sort of arguments are passed each time...  
![image.png](files/image_q.png)  
Alright, it looks like we're adding expr exactly once, it's in
`src/libcmd/installables.cc` Perhaps this means that we're actually getting
flags & soforth from a lot of different places for each command; that could make
it more difficult to modify properly as I'm not sure which commands take the
expr flag and I do not want to break any of them

- [ ] Check what commands take the expr flag, make sure that I do not break any
- of them  
   Regardless, here we are:

```
    addFlag({
        .longName = "expr",
        .description = "Interpret installables as attribute paths relative to the Nix expression *expr*.",
        .category = installablesCategory,
        .labels = {"expr"},
        .handler = {&expr}
    });
```

It has the same description as in `nix help shell` too, so it's very much more
likely to be the right one: ![image.png](files/image_d.png) Now, in the same
file there is a function called `parseInstallables` which appears to process the
expr and file flags. Let's see what happens if we provide an `expr`

```
std::vector<std::shared_ptr<Installable>> SourceExprCommand::parseInstallables(
    ref<Store> store, std::vector<std::string> ss)
{
    std::vector<std::shared_ptr<Installable>> result;

    if (readOnlyMode) {
        settings.readOnlyMode = true;
    }

    if (file || expr) {
        if (file && expr)
            // This code path is not followed when only expr is provided

        if (file) // This code path is not followed when only expr is provided

        auto state = getEvalState();
        auto vFile = state->allocValue();

        if (file == "-" || file) {
            // 2 code paths are not followed when only expr is provided
        else {
            auto e = state->parseExprFromString(*expr, absPath("."));
            state->eval(e, *vFile);
        }

        for (auto & s : ss) {  // I don't know what ss is: it's not a fantastic name
            auto [prefix, outputsSpec] = parseOutputsSpec(s);
            result.push_back(
                std::make_shared<InstallableAttrPath>(
                    state, *this, vFile,
                    prefix == "." ? "" : prefix,
                    outputsSpec));
        }

    } else {
  // This code path is not followed when expr is provided
    }

    return result;
}

```

- [ ] What is the `ss` variable in parseInstallables?

```
auto state = getEvalState();
auto vFile = state->allocValue();
// ...
auto e = state->parseExprFromString(*expr, absPath("."));
state->eval(e, *vFile);
```

This seems to be where the expression is evaluated  
Let's go through each line and see if we can figure out what's going on in a bit
more detail ![image.png](files/image_v.png) getEvalState seems to be defined
in `src/libcmd/command.cc` It's a function that returns a ref of EvalState (I'm
assuming ref stands for reference here and that I probably don't need to worry
too much about it)

```
ref<EvalState> EvalCommand::getEvalState()
{
    if (!evalState) {
        evalState =
            #if HAVE_BOEHMGC
            std::allocate_shared<EvalState>(traceable_allocator<EvalState>(),
                searchPath, getEvalStore(), getStore())
            #else
            std::make_shared<EvalState>(
                searchPath, getEvalStore(), getStore())
            #endif
            ;

        if (startReplOnEvalErrors) {
            evalState->debugRepl = &runRepl;
        };
    }
    return ref<EvalState>(evalState);
}
```

![image.png](files/image_1z.png)  
evalState appear to be defined in a number of places; I'm going to check out
`command.hh` first as I got here from `command.cc`
![image.png](files/image_6.png) And that was entirely unfruitful... Now I'm
going to go down the `#include` list and check out any other files there where
this is defined.

```
###include "command.hh"
###include "store-api.hh"
###include "local-fs-store.hh"
###include "derivations.hh"
###include "nixexpr.hh"
###include "profiles.hh"

###include <nlohmann/json.hpp>
```

Again, not a lot. Looking back at my grep output from earlier
`src/libexpr/eval.hh` seems to have an EvalState definition that's more than a
single line It's a long definition though, and it doesn't seem to be
particularly helpful so for brevity I have excluded it from these notes. It is
trivial to find again anyway. A similar exclusion was made for the corresponding
`.cc` code So, anyhow, back to this:

```
auto state = getEvalState();
auto vFile = state->allocValue();
// ...
auto e = state->parseExprFromString(*expr, absPath("."));
state->eval(e, *vFile);
```

The gist of this is that it takes the input expression and evaluates it.
Somehow, we'd like to change something like `nix shell nixpkgs#python3 --expr 'withPackages(...)'` into `nix shell --expr 'with builtins.getFlake "nixpkgs"; with legacyPackages.<system here>.python3; withPackages(...)' --impure` We have
the contents of expr; let's think about what we'll need to do to get it to
work... Firstly, by typing some random nonsense into nix shell we can see what
attributes nix is looking at when it looks at the flake
![image.png](files/image_3.png) Specifcially, it's
`packages.<system>.attribute`, `legacyPackages.<system>.attribute` and
`attribute` Let's head into nix repl and try some stuff out...
![image.png](files/image_g.png) Firstly, I'm going to try combining the with
statement for python3 into a single with statement. I expect this to work
![image.png](files/image_u.png) And it worked, fantastic! Now let's see what
happens if the attribute doesn't exist... ![image.png](files/image_2.png) It
doesn't break! Another fantastic sign And we have a similar effect if we try to
get a nonexistent attribute of a nonexistent attribute
![image.png](files/image_k.png) Therefore, my plan is this: I'm going to
transform the expression into something like the following

```
### nix shell flakename#attribute.on.that --expr 'expression'
with (builtins.getFlake "flakename").attribute.on.that; with (builtins.getFlake "flakename").legacyPackages.<system>.attribute.on.that; with (builtins.getFlake "flakename").packages.<system>.attribute.on.that; expression
```

Executing nix shell while providing a package along with the expr argument
currently yields an error; this new update will need to change that
![image.png](files/image_4.png) Further, we will need to be able to get the
packages that have been supplied where I'm doing my expression handling

So, a list of what we'll need then

- The current system (probably just using builtins.currentSystem)  
  ![image.png](files/image_n.png)
- A parsed list of flakes and atrributes supplied at the command line The expr
- argument, before execution and in such a way that we can change its value to
- the new value (i.e. readonly stuff is useless to us here)

Further using `builtins.currentSystem`,

```
### nix shell flakename#attribute.on.that --expr 'expression'
with (builtins.getFlake "flakename").attribute.on.that; with (builtins.getFlake "flakename").legacyPackages."${builtins.currentSystem}".attribute.on.that; with (builtins.getFlake "flakename").packages."${builtins.currentSystem}".attribute.on.that; expression
```

I guess the next thing to do then is figure out how we can get a parsed list of
flakes and atrributes supplied at the command line inside installables.cc To do
a quick test that everything will work I'm going to simply add nixpkgs#. to
scope like so:

```
            Strings e = {};
            e.push_back("with (builtins.getFlake \"nixpkgs\")");
            e.push_back(*expr);
            auto parsed = state->parseExprFromString(concatStringsSep(";", e), absPath("."));
            state->eval(parsed, *vFile);
```

Let's see if it works...  
![image.png](files/image_1r.png)  
ahhh, not quite  
![image.png](files/image_j.png)  
well it works admirably _with_ the impure flag; let's just make that the default
then...

```
-        if (file) evalSettings.pureEval = false;
+        evalSettings.pureEval = false;

```

much better  
![image.png](files/image_t.png)  
Now let's get the installables so we can include them in our expression

```
        for (auto & s : ss) {
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
                result.push_back(std::make_shared<InstallableFlake>(
                        this,
                        getEvalState(),
                        std::move(flakeRef),
                        fragment,
                        outputsSpec,
                        getDefaultFlakeAttrPaths(),
                        getDefaultFlakeAttrPathPrefixes(),
                        lockFlags));
                continue;
            } catch (...) {
                ex = std::current_exception();
            }

            std::rethrow_exception(ex);
        }

```

This part of the code runs if we do not provide an expr `flakeRef`, `fragment`
and `outputsSpec` are likely the name of the flake, the attribute path, and
something else Finding the definition with `rg`, `std::tuple<FlakeRef, std::string, OutputsSpec> parseFlakeRefWithFragmentAndOutputsSpec(` The types
are `FlakeRef`, `string` and `OutputsSpec`. OutputsSpec seems to be _either_
DefaultOutputs() _or_ AllOutputs() _or_ a list of attributes that we want to
get. This is interesting as it's (1) entirely undocumented and (2) currently
something that I'm not going to implement. FlakeRef has a `to\_string` method,
which I'll be using to convert it into a format I can use

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
e->eval(*state, env, *vFile);
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
e->eval(*state, env, *vFile);
```

Let's build and check that it works, and if it does we'll move on to the next
part.

### Fixing the errors

As you can probably tell by the fact that this section exists, it didn't work

```cpp
warning: Git tree '/home/minion/Code/c-cpp/nix' is dirty
error: builder for '/nix/store/pm3qfmizqhxhpc4cxsg6gdh3avlp5s7m-nix-2.12.0pre20220920_dirty.drv' fai
led with exit code 2;
       last 10 log lines:
       >       | ^~
       >   CXX    src/libexpr/attr-set.o
       >   CXX    src/libexpr/attr-path.o
       > In file included from src/libexpr/eval.cc:1:
       > src/libexpr/eval.hh:285:17: warning: inline function 'bool nix::EvalState::evalBool(nix::En
v&, nix::Expr*)' used but never defined
       >   285 |     inline bool evalBool(Env & env, Expr * e);
       >       |                 ^~~~~~~~
       >   CXX    src/nix/why-depends.o
       > make: *** [mk/patterns.mk:3: src/libexpr/eval.o] Error 1
       > make: *** Waiting for unfinished jobs....
       For full logs, run 'nix log /nix/store/pm3qfmizqhxhpc4cxsg6gdh3avlp5s7m-nix-2.12.0pre20220920
_dirty.drv'.
```

It turns out that I copied some markdown into a c++ file inadvertently, removing
this fixes the problem.

## Part 2: Getting the attrs
