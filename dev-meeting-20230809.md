Present: @rgrinberg @kit-ty-kate @Leonidas-from-XIV

`available` field
-----------------

* There is a PR for it, but do we want it?
* Do we want full support for all OPAM fields in the generator?
  - Dune can't do anything with `available`
  - So it can't do anything than just pass it on in the generator
  - People *already use* Dune as OPAM file generator
  - `available` in OPAM is used as a filter before being fed into the solver
* If Dune can't use `available`, why does it support `maintainers`?
  - Can be used in `dune subst`
  - But the explanation is admittedly a bit post-hoc
* How else to solve it?
  - Traverse *source tree* and collect `enabled_if`
  - But what if people forget to add `package` to their stanzas?
  - What if we went the other way and set `enabled_if` from `available`
  - Do we have the same variables
    * no, but we could start with a subset
    * we most likely won't have support for user variables
  - It's gonna be tough
    * The filter languages of OPAM and Dune don't map very well
    * The `enabled_if` filter would need to be extended
* Users expect Dune to write OPAM files
* It doesn't make sense to *support every opam field*
  - What goes into `dune-project` vs `dune-workspace`
  - What about other fields like `uninstall`
    * Packages can use the field to remove config files
    * How is this possible with Dune, given it generates an `.install` file
      - A binary in the package could generate the config file, then the `uninstall` would remove it. So it is not Dune/OPAM creating it.
* Why are OPAM files generated at all?
  - No good answer, but it is nice that there is a common useful subset for OPAM and Dune
  - The idea was that OPAM would be able to read the `dune-project` but so far this hasn't happened
  - Nix doesn't use the dune data but OPAM data
  - In the future it could use the info from the lock dir
  - At least this way users only need to know Dune sexp syntax and not OPAM syntax on top of it
* Would `available`/`enabled_if` support be a *helpful addition* to opam-repository maintainers?
  - Kate says it would be because some packages are missing `available` and maintainers need to add it by hand.
* Conclusions
  - We agree that it is useful to have the metadata in `dune-project` checked within the build to verify that it somehow makes sense. A bad example is `depends` which is there but can also be completely wrong.