# Agenda
- https://github.com/ocaml/dune/pull/12800 (@Alizter)
- 3.21 regressions (@Alizter)
  - https://github.com/ocaml/dune/issues/12619

# Notes

- present: @alizter, @electraas, @sudha247, @shonfeder, @art-w, @Leonidas-from-XIV

- https://github.com/ocaml/dune/pull/12800 (@Alizter)
  - Building oxcaml with dune, we set `INSIDE_DUNE`
  - This limits concurrency of dune to 1
  - So oxcaml only builds with 1 core and it takes like 1/2 hour for it to build
  - Hack is to unset `INSIDE_DUNE` in opam file
  - Propper fix is to decouple `INSIDE_DUNE` from setting concurrency
  - There is a PR in place with two commits:
    - We add an env var to set concurrency
    - We change `INSIDE_DUNE` to no longer determine concurrency
    - Requires setting a lot of tests to use the new concurrency env var
    - But this is creating a lot of noise in test suite due to how server works
  - Ali: has suggested putting this in release, (and blocking on it)
    - After discussion we agreed not to increase scope of the release, but that we would aim to get this in ASAP.
- 3.21 regressions and release (@Alizter)
  - https://github.com/ocaml/dune/issues/12619
  - Need a fix for opam-dune-lint (https://github.com/ocaml/dune/issues/12897)
    - If no one is using this in critical infra, then we can drop an upper bound on opam-dune-lint
  - Dune hangs on mdx tests https://github.com/ocaml/dune/issues/12900
    - This is a bug, but not something we necessarily *need* to block on the 
    - Amber is working on the fix, we think it should
  - Fixed two critical braeking regressions
    - Breakage on alpine and freebsd: needed inlined macro
    - Ali rewrote cram lexer, which broke a single test in opam-repo-ci
  - Shon: I propose we only release what we have now, just fixing any critical regressions, main
  - https://github.com/search?q=%22INSIDE_DUNE%22&type=code
  - There is a new `file` stanza we would like to be in release `3.21`
    - Shon: will cut new release branch
- User reaserch and feedback (@ElectrAAS) 
  - Got a lot of good feedback at Tarides holiday party

- Action items
  - [x] Shon: Reviewing the js_of_ocaml PR -- actually, there is a blocking review comment there now
  - [x] Shon: Merging the inline tests
