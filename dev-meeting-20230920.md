Present:
@emillon
@gridbugs
@Leonidas-from-XIV
@rgrinberg
@rikusilvola
@rjbou

- 3.11 blockers (@emillon)
  - missing generated opam file
    - why is opam file generation attached to `@install`? because they are installed. why are they? open an issue to discuss 
    - fix: make the rule fallback instead of removing it (#8706). we should try to make it consistent with other changes to ignore-promoted-rules. open an issue to rename the CLI flag
  - ctypes 0.1 and 0.2 removal
    - breakage was expected but this affects many packages transitively
    - we make sure that the experimental status is well documented and proceed with the change (adding upper bounds and notifying maintainers)

- repository management (@Leonidas-from-XIV)
  - feature in #8633: more eyes welcome regarding design

- 4.0: release management vs publicity (@emillon)
  - risk of tying new features (package management) and breaking changes (various removals/renames) in 4.0
  - see ocaml 5 (multicore added, but difficult to try for unrelated reasons: removal of unprefixed functions and stdlib modules)
  - we can introduce package management in a classic 3.x.0 release

- package management status (@rgrinberg)
  - we can now use binaries and libraries built in packages
  - post dependencies are missing (used in compilers) to use that for Real World:tm: use cases.
  - we'll need to add constraints in the lock file regarding the version of dune (be consistent with the current dune binary)