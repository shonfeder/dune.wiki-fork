Present:
@alizter
@emillon
@Leonidas-from-XIV
@rgrinberg
@rikusilvola

- [opam synopsis](https://github.com/ocaml/dune/pull/8850)
  - we agree about the non-synopsis changes in that PR
  - 1 terms is causing issues:
    - "opinionated" sort of implies that "the dune way" is incompatible with the rest of the ecosystem (misleading)
  - both the README and dune.build say "A composable build system for OCaml"
    - we note that "composable" is not a beginner feature and needs to be define
    - "for OCaml" shadows the fact that Dune can also build C, C++, and Coq.
  - short term solution: we reuse the tagline from the readme and the website
  - later we can have a discussion about changing it to something more precise

- 3.12 release
  - early to mid November?
  - one goal is to avoid the freeze in December, so if we want 2 releases in Q4 it's better to spread them out

- package management: what's missing to execute build plan?
  - building the compiler
  - some packages like zarith don't build