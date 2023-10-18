present: [@Alizter](/Alizter), [@Leonidas-from-XIV](/Leonidas-from-XIV), [@rjbou](/rjbou), [@rikusilvola](/rikusilvola)

* Dune opinionated: should we change the headline to something else?
  * [ocaml/dune#850](/ocaml/dune/pull/8850)
  * Pros: 
    * Concerns about this scaring away people - is this really a selling point?
    * Could still be mentioned later in the docs!
  * Cons:
     * Removing the mention could reduce our ability to say no to new workflows
     * Removing the mention could reduce our ability to make opinionated decisions
     * Are those real concerns?
  * To be discussed further on the issue and on Slack

* Dune pkg not working with Zarith
  * [ocaml/dune#8931](/ocaml/dune/issues/8931)
  * Issue with use of `ocamlfind install` not working
  * Widely used package that needs to be supported
  * Rather not maintain an overlay, or a workaround in Dune itself (increasing toil)
  * Adding prefix variable in opam file to be investigated

* Discussion on definition of Windows support
  * Platform Roadmap states that we should aim to provide same functionality on the Tier1 OCaml platforms (with caveats e.g. sandboxing)
  * Dune itself, nor any platform tool team, is responsible for other tools being Windows compatible (e.g. compiler)

* Two new package management commands
  * `dune pkg outdated` for seeing outdated packages in the lock file 
  * `dune describe pkg lock` for seeing lockfile contents without ls-ing files in the lock folder