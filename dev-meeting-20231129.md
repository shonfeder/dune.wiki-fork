## 3.12.0 release postmortem; release process: present, future (@emillon)

- in our [current release process](Release-process), we make minor releases (like 3.12.0) off `main`. Because there was a relatively long time between the time `3.12.0-alpha` branched off `main` and the time `3.12.0` got released, some changes not intended for 3.12.0 had been merged in main. So 3.12.0 contains changes that know about 3.13 language.
- 3.12.0 should not be used. it has not been published to opam-repository. 3.12.1 has been created (on the usual 3.12 branch) with reverts to the problematic PRs.
- moving to a release branch model should prevent this: instead of making the release from main, we do it from a dedicated branch (equivalent to the one we call 3.12.0-alpha today)
  - @nojb: this is how ocaml/ocaml and many other projects do it
  - @rgrinberg: releasing should not disrupt the dev cycle too much
  - @Leonidas-from-XIV: branching times should be communicated in advance
- we will try this approach for 3.13

## [Examples directory](https://github.com/ocaml/dune/issues/9287) (@Alizter)
- our `examples/` directory is confusing because it uses cram syntax, but is marked as data only so they don't run
- this piece of feedback comes from a new user so this is rare and valuable feedback
- how do we expect these examples to be interacted with?
  - it's useful to have them in a terminal and play with them
- examples in the rst docs serve a similar purpose
- @emillon to write docs about cram tests

## [Specifying branches for repos](https://github.com/ocaml/dune/pull/9241) (@Leonidas-from-XIV)
- there's an ambiguity if a name like a branch is passed: to resolve it, it is necessary to connect to the git repo
- when name is a branch or tag, a call to the network is expected (and lockfile will record just commit)