## Agenda

- Merging strategy (@maiste)
- CI broken on main (@Leonidas-from-XIV)
- Signal to use to enable `dune pkg` (@Leonidas-from-XIV)
- Caching ocaml-toolchain (@Alizter)
- Lock dir representation (@Leonidas-from-XIV)

## Meeting notes

Attendees: @Alizter, @maiste, @Leonidas-from-XIV, @OlivierNicole @shonfeder @ElectreAAS @aguluman @rgrinberg @Sudha247

### Merging strategy (@maiste)

- The history is hard to read because of merging from main into branches and merging the branches back to main
- Rebasing takes a while as we need to wait for the result before merging
  * This is @Leonidas-from-XIV workflow, nobody enjoys this
- Merge queue was tested and didn't work well
- Automerging?
  * @Leonidas-from-XIV tested it and it merged immediately, so it didn't work well
  * @shonfeder points out that the tests are not configured as required, thus it didn't wait for them to complete
- We should try automerging in the future

### CI failures (@Leonidas-from-XIV)

- CI was failing on `main` a lot lately
- Often times this is due to Melange or Jsoo, so please make sure they don't break the build
- It makes contributing hard because PRs branched of from broken `main` inherit the brokenness and need to be rebased after `main` is fixed, creating unnecessary friction for contributors
- Some test lock up forever
  * Rudi was able to reproduce one locally and disabled it
- Ali wants to implement a timeout for cram tests, so they fail on tests that get stuck

### Signal to enable Dune package management (@Leonidas-from-XIV)

- If we autolock into `_build` without promotion into the source tree we can't use the presence of a lock dir in a project as a signal to enable package management
- New flag in `dune-workspace`
  * That requires everyone who wants to use it create or edit their `dune-workspace`
- Additionally a flag in dune config, so it can be enabled globally for all user's projects
- It will still allow installation via OPAM without package management as `-p` disables lock directories.

### Caching the toolchain (@Alizter)

- OCaml toolchain is getting invalidated when you delete `_build`
  * One option is to not do that
  * However, what about CI setups?
    + Why not share `_build` as well?
    + Not great if the same CI setup is building multiple projects that should all reference the same toolchain compiler
- Make the toolchain rule put the result in the shared cache instead of recompiling it
- The compiler can't be put in the shared cache itself because it is not relocatable and restoring it from the cache leads to a non-working compiler
- If we put the cookie in the cache it is possible that the compiler it points to gets deleted, thus it would break the cache
- We'd need to store it in the cache and make sure that what it references still exists
- How do we know a file in the cache is a cookie?
  * The cache is content-addressable so we can't ask for the file name
  * The cookies need to register their existence somewhere from where they can be deleted
- We could write a command that will clean up toolchains and fix up broken cookies
- When is the relocatable compiler coming?
  * @rgrinberg: The feature is basically ready and according to @dra27 going to be backported
  * @OlivierNicole: it's scheduled for OCaml 5.5 by the end of the year, no idea about the backporting to older compiler

### Autolocking API (@Leonidas-from-XIV)

- Currently it is all `Path.Source.t` but if we write/load in `_build`, should it be `Path.t` or `Path.Local.t`?
- @rgrinberg At best it would be `Path.Build.t` since that's where they will live
- It the lock dir is in the source directory, it can be copied to `_build` anyway, like any other source file for building so we can just read it from there
- Maybe we should make people store just inputs to the solver
- This avoids issues with portable lock files as the solver will find a configuration for the current machine
- @maiste: What should be our principal, recommended workflow for `dune pkg`?
  * We don't really know yet
- We could in theory support he opam lockfile format
  * There's some issues with that as the lockfile format still has the filters on and our lock format resolves filters but it is pretty close

### Native `cflags` (@OlivierNicole)

- https://github.com/ocaml/dune/issues/11923
- Blocks Semgrep
- @rgrinberg does not have this on his roadmap, unsure about the Tarides roadmap for this
- @voodoos worked on building the stubs concurrently
- @shonfeder: What is needed to solve this?
  - @OlivierNicole: Dune should do the right thing by default, because currently Dune users need to configure it manually to work properly, but it can be worked around
- @rgrinberg: It's definitely a bug and Dune should do the right thing by default especially as @voodoos has implemented the necessary support already to make it work properly