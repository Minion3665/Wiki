| Project  | Ambitiousness   | Previous similar stuff  | Interest                            | Potential issues                                                   |
| -------- | --------------- | ----------------------- | ----------------------------------- | ------------------------------------------------------------------ |
| UTTT AI  | Quite ambitious | None, Ash made TTT ML   | Quite (if ML), Not so much (if not) | I don't know how to do ML; I can't do rust and integrate with UTTT |
| Kcaputt  | Extremely so    | Unity multiplayer stuff | A bit; sadly not so useful now      | I don't care. Multiplayer stuff is dogshit. UI would be needed     |
| Task man | Sortof so       | timelog.sh              | Could be useful                     | Maybe not ambitious enough                                         |

Here are a few ideas for my NEA.

Stuff I don't want to do

- Stuff related to security, as I think that if I ever want to use it in the
  future this will become a problem to maintain
- Utilities that I won't use

Task manager is quite a good one, as it could integrate with stuff like
Collabora's chronophage etc., and be in a language I like such as rust. I have
quite an expansive idea for this too due to my EPQ, so there's a possibility for
growth. I think I would use it more than a UTTT AI too, so that's a potential
advantage.

Regardless, I need to submit 2 project ideas, so I will write up and submit both
the UTTT AI and the Task manager. If the task manager gets chosen, I will write
in rust. If the UTTT AI gets chosen, I am bound do to it in python (if ML) or
Typescript (if I want it to work with my existing UTTT implementation (which I
might). Either way rust doesn't seem like the smartest choice.

# Primary project proposal

- Name: Skyler Grey
- Class: CG-Y2-21-B01
- Project title: Task Manager
- Description & Aims: I often find myself having to do _stuff_. It regularly
  happens that when I am doing stuff I want to know what stuff I have to do,
  and how long I've spent doing the stuff etc. This project aims to write a
  task tracker to manage that, with a TUI dashboard to show you what stuff
  you really really need to do. This was partially conceived during my NEA so
  I also had some plans that would be able to force you to write
  documentation/weekly reports etc. on certain days. I still think this is a
  good idea, but I now think it'd be more in the context of a business where
  you have to report progress (i.e. weekly reports of what you've been up to).
  It's possible although not necessary that I may try to integrate with other
  services, i.e. MS Teams to track homework assignments
- Potential client/3p/EU: Someone who both (1) spends a lot of time in the
  terminal (so a TUI is most useful to them) and (2) has responsibilities
  (perhaps they are a student or have a job or need to take medications).
  Preferably they would also (3) not have a good organization system in place
  already or be dissatisfied with everything they've tried damn you
  taskwarrior I thought you'd save me
- Access to client?: I am someone who fits this, I have friends who fit point 2
  and either 1 or 3, although I don't know if I know someone who fits all 3
  points
- What programming skills can this demo?:
  - Database (storing tasks), multi-tables (tasks, projects etc.)
  - Lists (tasklist)
  - Perhaps a queue if you really fancy doing that (although there may have to
    be some way to dequeue from the middle as you might complete a task out of
    sequence)
  - Polymorphism maybe? It really depends on how I impl some of the required
    task stuff or tasks that have commands
  - Simple business logic
  - Calling web APIs (maybe, depends on if I impl teams and/or chronophage)
  - Sorting (merge sort because it's my favorite <3)
  - Fuzzy finding? (could use fzf instead of self-impl though)
  - TUI (cool I guess)
  - Possible plugin system (I would _really_ like to have teams and chronophage
    impl-ed as extensions rather than in core, that way I could easily switch
    over to a university system/another job/other people could use it without
    having stuff that I added get in their way)
- What data will my system store?:
  - Tasks
  - Projects
  - Credentials to web services
- Have you prototyped part of the project?: Not directly, although I have made
  some of these things (TUIs, basic time tracking etc. before, so I have some
  ideas of how they should be made)
- Useful links
  - https://gtimelog.org/
  - https://taskwarrior.org/
  - https://docs.microsoft.com/en-us/graph/education-concept-overview
  - https://wiki.collabora.com/Chronophage/gTimeLog
  - https://github.com/fdehau/tui-rs
  - https://github.com/kdheepak/taskwarrior-tui

# Alternate project proposal

- Name: Skyler Grey
- Class: CG-Y2-21-B01
- Project title: UTTT AI
- Description & Aims: I like ultimate tic tac toe a lot more than my friends do.
  Unfortunately, this means that I often don't have someone to play with :(.
  This project would make a nonhuman friend to play UTTT with me, starting
  with a simple greedy algorithm and potentially stepping up to machine
  learning.
- Potential client/3p/EU: Someone who likes playing ultimate tic tac toe but
  doesn't always get the opportunity to play with people
- Access to client?: I am someone who fits this, I have friends who know how to
  play UTTT and would probably be willing to playtest
- What programming skills can this demo?:
  - Algorithms (sorting, greedy, possibly pathfinding stuff depending on impl)
  - MultiD arrays
  - Possibly Polymorphism (depending on impl of Algorithms, seems a lot more
    likely than tasklist)
  - Recursive algos (To evaluate the position I...)
- What data will my system store?:
  - Ephemerally, board states. Long-term: training data if AI, otherwise none
- Have you prototyped part of the project?: Yes. I have ultimate tic tac toe
  made. It is hosted at https://uttt.crawling.us and will be for the
  forseeable future
- Useful links
  - https://uttt.crawling.us
  - https://bejofo.net/ttt
  - https://mathwithbaddrawings.com/2013/06/16/ultimate-tic-tac-toe/
  - https://www.freecodecamp.org/news/simple-chess-ai-step-by-step-1d55a9266977/
  - https://towardsdatascience.com/implementing-a-deep-learning-chess-engine-from-scratch-275e5c9b9e21
