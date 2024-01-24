Present: @rgrinberg @Leonidas-from-XIV @moyodiallo @ElectreAAS Maxim Grankin, Boning Dong

* Changes to `install` in 3.11, Maxim
  - In 3.11 we deprecated installing to parent directories (`../`)
  - How long will the functionality be still available? Until 4.0 at least
  - As an alternative to this, can new `sections` be added?
    - `sections` are mostly inherited from OPAM, for compatibility
    - Files can be installed in the prefix root (`dune install --prefix=DIR`)
    - New sections with a flag as guard welcome to be submitted as PR

* Submodule support (@Leonidas-from-XIV)
  - Submitted
  - Missing support for relative file paths, although that is not a common use case (the Git CLI has guards against doing this) and if people need it it can still be added

* Git support (@rgrinberg)
  - We need to make sure packages that have Git sources use the Rev Store
  - @Leonidas-from-XIV to check that this is happening

* Missing items for the MVP (@Leonidas-from-XIV)
  - Pinning
  - Relocatable ocamlfind (@emillon would know about the progress on this)
  - Performance improvements
  - We are in good shape!
  - @rgrinberg has a list of [outstanding issues](https://github.com/ocaml/dune/issues?q=is%3Aopen+is%3Aissue+label%3A%22package+management%22) that can be picked up

* Opam-repository changes (@Leonidas-from-XIV)
  - @Leonidas-from-XIV attended the meeting about scaling the opam-repo [ocaml/opam-repository#23789](https://github.com/ocaml/opam-repository/issues/23789)
  - The proposal with currently the biggest amounts of proponents puts archived packages in a secondary repo
  - No issues for the dune packaging support

* Duplicate packages in multiple repos (@ElectreAAS)
  - @ElectreAAS wants to fix a part of the packaging code that @Leonidas-from-XIV was concerned about
  - We pass a list of valid package candidates to the solver in the order that the user specified the repositories in
  - The solver would give us a package + version and to determine which repo it was from we would need to search for it
  - This could potentially create issues where the package we find and the package the solver wanted are different if name + version are the same
  - @rgrinberg fixed this by never giving passing the solver the same name + version twice
  - OPAM behavior in this case is unspecified
  - It would be nice to have a test to check this behavior 
