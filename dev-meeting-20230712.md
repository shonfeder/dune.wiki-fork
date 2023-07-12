Present:
@alizter
@ejgallego
@emillon
@leonidas-from-xiv
@lubegasimon
@nojb
@rikusilvola
@rjbou
@rgrinberg

- which shell to use in cram tests (#8134, @emillon)
  - most useful part is to be explicit about bashisms
  - on windows, it makes us rely on cygwin
  - today we use `which sh`
  - possible way to make smaller feature: just allow bash?
  - we want to make users write portable tests but we struggle with that ourselves

- dependency loops when computing artifacts (#7908, @ejgallego)
  - to create install rules, we depend on running binaries
  - melange/coq config technique: use build_system API which can work in some cases
  - resolve_program computes all installable artifacts to get the artifact db. computing the executable and library artifact db separately can be enough.
  - we could be more explicit and ask where the binary is expected to come from to avoid loading everything

- [rule streaming](https://github.com/ocaml/dune/blob/f535d4cb4adc46f7e214a6878d9aaa10003dc86c/doc/dev/rule-streaming.md) (@rjbou)
  - background: we have rules for each package, and also rules that use these. ex: ocaml is built, but also used in other packages.
  - in dune toolchain info is done during context creation
  - rule streaming solves this
  - staging things across contexts works: either the compiler vs the rest; or packages vs user rules
  - send feedback on RFC

- Roadmap of packaging features that we want to work on next (@leonidas-from-xiv)
  - some info missing on the bugtracker
  - possible features: build instructions in the lockfile, implementation of filters, opam substitution
  - start by creating tests: how these opam files should be turned into lock files
  - how to get hash of opam-repository used when locking?

- we didn't get to talk about:
  - benchmark bot (@emillon)
  - dune monitor command (#8152, @alizter)