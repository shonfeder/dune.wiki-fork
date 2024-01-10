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

- 3.13 branching and updated release process (@emillon)
  - we now use the same branch for alpha versions and `x.y.0`
  - development can continue on `main` while the release is being done
    - just be careful in case some changes need to be backported
    - in particular, unrelated subsystems (e.g. coq) can continue being developed

- single-command bootstrap (@emillon)
  - pointers: https://github.com/ocaml/dune/pull/9613 https://github.com/ocaml/dune/issues/9507 https://github.com/ocaml/dune/pull/9563
  - bootstrap process can use 2 strategies:
    - parallel: run compile commands in parallel and link the rest
    - single-command: run ocamlopt with a ton of arguments
  - today single is used if win32 or if `-j 1` is set (implicitly or explicitly)
  - problem: parallel and single-command create different binaries
  - each strategy is reproducible though
  - assumption was that single would be faster than j1 on linux and jx on windows
    - from a quick benchmark the assumption is true on linux but false on windows
  - we lean towards removing that code path to simplify the bootstrap process and fix reproducibility issues

- opam-repository scaling (@gridbugs)
  - opam-repository issue about scaling the repository is relevant to what we're doing in dune: https://github.com/ocaml/opam-repository/issues/23789