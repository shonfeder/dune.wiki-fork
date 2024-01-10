Present:
@emillon
@gridbugs
@Leonidas-from-XIV
@moyodiallo
@nojb

- Expansion in `(modules)` field (@nojb)
  - https://github.com/ocaml/dune/pull/9578
  - dynamic forms like `(:include)` or `%{read-lines}` can be used in `(modules)`
  - the included file needs to come from a different directory
  - the error message does not provide a good hint
  - other places with this kind of dynamic dependencies have the same characteristics
  - this can replace some usages of ocaml syntax
  - used at Lexifi for a sort of static plugin structure (modules selected at compile time)
  - this feature will be advertised in ocaml.org changelog

- opam-compatible package name validation (@Leonidas-from-XIV)
  - we added extra checks for `(depends)` in cases where opam generation isn't used
  - some packages have errors in there but we're not sure how many
  - include check in `3.13.0~alpha1` and act depending on results
  - we can add bounds on affected packages if there are not too many
  - otherwise we can only perform the check (and type conversion) at use: when generating opam files and in pkg rules

- 3.13 branching and updated release process
  - post description on channel

- single-command bootstrap (https://github.com/ocaml/dune/pull/9613 https://github.com/ocaml/dune/issues/9507 https://github.com/ocaml/dune/pull/9563)
post summary notes

- steve
opam https://github.com/ocaml/opam-repository/issues/23789

Agenda
- single-command bootstrap (https://github.com/ocaml/dune/pull/9613 https://github.com/ocaml/dune/issues/9507 https://github.com/ocaml/dune/pull/9563)
- 3.13 branching and updated release process
- https://github.com/ocaml/dune/pull/9578 
- opam-compatible package name validation