Present:
@alizter
@emillon
@gridbugs
@leonidas-from-xiv
@nojb
@rjbou

## Release blockers
- #9009 review (many changes in the PR)
- #9176 - we can go for a quick fix but it would be better to have a better understanding of the `""` default and more tests will be useful

## Reproducibility issue (#9152)
- @nojb reproduced this with just `make`
- still unclear what happens, maybe a bug in the compiler driver, can we capture the missing dependency in dune?

## cmd files on Windows (#9128)
- execution of some scripts (cmd and bat files) differ on Linux and Windows
- on Windows, `CreateProcess` is not able to run them directly, the shell is responsible for this
- dune files that use `(run)` are not portable to windows if the program happens to be a script (ex: `yarn.cmd`)
- it's possible to switch to `(system)` but it's a detail that shouldn't impact behaviour on Linux
- we could special case `(run)` to execute cmd/bat files. but we're concerned that we're injecting magic in a place difficult to debug
- we agree that something could be done but we don't have enough data to take the right decision
- we'll make sure that the behavior on windows is properly documented, and defer the fix to later (we expect more windows users later)

## divergence of our opam fork
- the changes to the opam libs are not being upstreamed
- some changes might not be mergeable
- opam team is open to merging changes if that improves the lib but prefer that the changes are stable before accepting patches

## using pkg rules to control our vendoring
- this could be possible, but we don't think that this workflow is going to be useful for other users.