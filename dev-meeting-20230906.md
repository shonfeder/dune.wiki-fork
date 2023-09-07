Present:
@alizter
@emillon
@Leonidas-from-XIV
@nojb
@rgrinberg
@rikusilvola
@rjbou

## 3.11 release (@emillon)

- Starting next friday (2023-09-15)
- curious about breakage caused by removing old versions of ctypes
- depending on amount/which packages are broken we can adjust the error message or just send bound updates + PRs

## paths in dune-workspace for opam-repositories (@Leonidas-from-XIV)

- should we accept paths there?
- we should accept all kinds of urls, in the same format as opam does
- semantics in case of multi repositories are same as opam

### demo of `--display tui` (@alizter)

- supports folding
- adapts to terminal size
- in `dune build`
- in `dune monitor`
- supports tabs
- displays (internal) jobs
- uses `notty` and `nottui`
- team is invited to test the feature
- the long-term idea is to deprecate `build` and instead to connect to a monitor (to avoid lockfile issues when multiple dunes run in the same workspace)

### New OPAM repository format

- The new format might not unpack into separate `pkg/pkg.ver/opam` files
- The OPAM API should work transparently with either format
- OPAM will _always support_ the old format
- Unpacking and creating the files is slow, not downloading them. Especially on Windows
- In Dune we prefer the repos to be in Git for reproducibility, so as long as we can refer to the state by some hash it is ok for us