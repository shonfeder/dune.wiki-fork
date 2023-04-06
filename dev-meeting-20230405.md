present:
@alizter
@arozovyk
@emillon
@kit-ty-kate
@leonidas-from-xiv
@lthls
@nojb
@rgrinberg

- @emillon to update link in invite
- 3.7.1 release
  - still running CI
  - already in nix
  - next steps: announce on discuss, merge changelog in main
- dune in nix
  - dune 2 being phased out
- 3.8 release
  - starting the work in 2 weeks
- opam/dune integration (@leonidas-from-xiv)
  - what source fetching is about: curl and unpack in a directory
  - opam is a large dependencies and has dependencies itself
  - after keeping just the minimum, it's still +21kloc
  - if we use the solver etc we won't be able to tree shake a ton
  - @nojb: raises concerns about size + how to vendor properly
  - discussions about what goes in the switch: dependencies, compiler? -> only per compiler version. no `_opam` directory
  - if we're just fetching, we have the same limitations as opam-monorepo
  - @nojb: second approach is building and "installing" dependencies.
  - @rgrinberg: we'd need new rules about when to rebuild packages
  - @rgrinberg to write a proposal. packages are built in a directory named after the hash of its deps (sandboxed) in a content adressable way
  - @alizter: for projects depend on dune. do dependencies fetch dune? -> in monorepo it's special cased
  - what about multi-context builds (x-compilation or multi-version)? @rgrinberg: for v1, no difference between dune and non dune packages
  - integration strategy
    - @rgrinberg raises concerns about rebase costs
    - @emillon raises concerts about binary size and revertability
    - we agree that it's good to explore this
  - end status:
    - dune and opam are going to coexist
    - opam-repository to continue existing
    - we can upstream changes to opam
    - opam team on board
  - question about dune build - does it fetch sources by itself?
    - we want to avoid commands that ask to run a command
    - source fetching is just a rule with no dependencies
- [dependency work](https://github.com/ocaml/dune/pull/7498
) (@arozovyk @lthls)
  - questions about comparing names between dune libraries and main module of library
  - would need to build a map
  - cannot really do that for dependencies that do not use `dune` but this probably not an issue for these