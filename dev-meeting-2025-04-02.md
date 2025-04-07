## Agenda

- Getting `dune exec` to update `argv.(0)` in a sensible way. (@Alizter) (5 min)
  - https://github.com/ocaml/dune/pull/11559
  - addresses https://github.com/ocaml/dune/issues/11527

- Quality of life improvements for tests (@Alizter) (5 min)
  - Aliases for inline tests https://github.com/ocaml/dune/pull/11109
  - Aliases for `(tests)` stanzas https://github.com/ocaml/dune/pull/11558

- Fixed double-running actions. (@Alizter) (5-10 min)
  - cram tests being run twice https://github.com/ocaml/dune/pull/11547
  - user actions being run twice https://github.com/ocaml/dune/pull/11557

- `dune exec -w` leaving orphan processes. (@Alizter) (10 min)
  - https://github.com/ocaml/dune/issues/11089
  - Reproduction https://github.com/ocaml/dune/pull/11562

- Documentation on tests is messy and hard to find. (@Alizter) (10-15 min)
  - We should split the how-to and the reference
  - Many issues due to people unable to read the poor documentation

- Watch-mode with auto locking. (@maiste)

## Meeting Notes

Attendees: @Alizter @ElectreAAS @gridbugs @prometheansacrifice @aguluman @maiste @shym

### Dune exec

- @Alizter
    - `argv.(0)` is not rightly set when running `dune exec <program>`.
    - Convention in UNIX is to set it so it respect `<cwd>/<argv.(0)>`.

Another person should take a look at [the PR](https://github.com/ocaml/dune/pull/11559)

### Improvements with tests

- @Alizter
    - New users struggle with tests because there are a lot of way to do them.
    - Aliases for inline test:
        - It does a clever partionning using `ppx_expect` between the file and the tests.
        - This is used inside of Dune itself.
        - With inline-test, you are testing everything, there is no way to specify a particular area to test.
        - The solution is to attach the test to an alias with the library name.
        - See [PR](https://github.com/ocaml/dune/pull/11109)
    - Aliases using the `(tests)` :
        - It is possible to define aliases for tests when declared inside `(tests)`.
        - There are edge cases with `enabled_if`.
        - See [PR](https://github.com/ocaml/dune/pull/11558)
- @gridbugs:
    - Is it possible to remove the code from `ppx_expect` before providing it to the compiler?
        - Not at runtime because you would have to have `ppx_expect` declared as a library.
- @maiste:
    - Attached to the name of the library with other alias?
        - This is something that users can do. It is their responsibility if they want to override the default behavior

### Fixed double running action

- @Alizter
    - If you attached a `cram` tests to an alias, it will run the tests twice because the digest is different:
        - One alias is the main alias and the other alias depends on this alias. It prevents the computation from running multiple time.
        - See [PR](https://github.com/ocaml/dune/pull/11547)
    - User actions with multiple alias encountered the same problem:
        - Using `(aliases)` trying to build all the aliases within `dune build` runes the action twice?
        - Don't use `Super_context` but more `Rule` things.
        - Solution is almost the same: it attached the first alias as a dependency of the other aliases.
        - See [PR](https://github.com/ocaml/dune/pull/11557)

### Dune exec -w leaving orphan processes

- @Alizter
    - Internally we use `spawn` to create the processes.
    - If you kill `dune` while it running a subprocess, it will detach the orphan because `fiber` is not award of `spawn` processes.
    - Test for watch-mode are flaky and disable by default: remember to enable them if you need to work on it.
    - This is really hard to test.
    - The PID spawned by `spawn` is bind to a variable but, it is never used.
    - There is no clean up regarding `fiber`: only the scheduler is doing a cleaning stage.
-@gridbugs
    - Is it possible to use the Linux processes hierarchy?
        - It only works on Unix not on Windows.
        - We can't because it would penalize Windows users, and we don't want to make them second class citizen.

### Documentation on tests is messy and hard to find

- @Alizter
    - This is one of the most terrible part of the documentation.
    - It is hard to read for new users and, they end giving up dune.
    - A solution would be to write a reference page and link to specific how-to pages.
    - Tests are hard because there are multiple of workflow.
- @gridbugs
    - Documentation on Dune is hard because it is hard to have a mental model of Dune.
- @maiste
    - We could also write some tutorials to cover some workflows and give more step-by-step instructions.
- @Alizter
    - The problem with Dune comes from the "frontend" not the "engine": what do we expose to users?.

### Watch-mode

- @maiste
    - The current implementation requires to manually run `dune pkg lock` when the `dune-project` is changed.
    - One solution is to implement the auto-locking using the already known version of the packages we have installed. If you add a new package or change the version of one package, we would fix this version and run the solver with fixed version for every package apart the changed/newly-added one. If the solver doesn't succeed, it would ask to manually run the solver with `dune pkg lock`.

- @Alizter
    - We should add a command line flag to opt in the feature (`--auto-lock`).
    - It would be useful when bootstrapping a project, but you wouldn't want to relock every time on your project is at a stable point.
    - We want it to be optional because locking is slow and if you don't have a fast internet it is painful to do it every time.

### Questions about package management

- @Alizter
    - We should dog food Dune with Package Management.
- @gridbugs
    - In / out bug: project than depends on a project that depends on the project itself is not possible.
- @Alizter
    - It would be useful to get feedback on the feature from ourselves.
- @gridbugs
    - Could we change the name to build dune but without having a problem with it?
- @Alizter
    - Should we use the bootstrap version to build dune internally?
    - It would be needed before releasing package management as a stable system: how can we ask people to use something we don't use ourselves?
(- @maiste (not mentioned in the dev meeting but for future reference)
    - We (at Tarides) plan to use package management on the OCaml Platform to show people it can be used for development)
- @gridbugs
    - Portable lockfiles would be required to be able to release to the public.
-@Alizter
    - Windows support would be important to go public. We still don't want Windows users to feel like second class citizen.
    - Currently, there are symlink errors with WSL1: some detection might be required when building. See [issue](https://github.com/ocaml/dune/issues/11530)

