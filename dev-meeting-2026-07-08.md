# Agenda

- Which compiler variants to use in our Nix builds. frame-pointers, Flambda? (@Leonidas-from-XIV)
- Release 3.24.1 (@shonfeder)
- Release 3.25 (@alizter)
- Sustainability of dev practices and releases (@shonfeder)
 

# Meeting notes

Attendees: Rudi, Shon, Ali, Chukwuma, Marek, Sudha

- Which compiler variants to use in our Nix builds. frame-pointers, Flambda? (@Leonidas-from-XIV)
  - nix packages are always using flambda
  - but in dune builds we sometimes disable flambda but enable frame pointer
    - result is different OSs get different options (e.g., frame-pointers on macos, flambda on linux)
    - this seems inconsistent, how can we make this consistent accross systems?
  - Ali:
    - dev shells in dune code base use frame-pointers, useful for dev/debugging purposes
    - for binaries we are distributing to people, we should be using flambda, b/c will improve perf
  - Rudi:
    - are people aware that flambda is unmaintained? -- it has various bugs (e.g., on ARM)
    - only oxcaml flambda is maintained
    - so I do not recommend using it, until it is being maintained, as indicated by compiler devs
  - Rudi: binaries with frame-pointers are the default on some systems (macos, centos)
    - Ali: but cannot be added for windows
  - conclusion: disable flambda for the binary builds and for nix overlays
    - will require discussion with Antonio for nix overlays
  - Ali: could consider getting oxcaml to build the binaries, then could safely opt in to a (afawk) bug-free flambda
  - Rudi and Ali: could add separate melange CI job?
- Release 3.24.1 (@shonfeder)
  - https://github.com/ocaml/dune/issues/15439
  - should be able to cut the patch release in the next day or two
- Release 3.25 (@alizter)
  - Some stuff needs to be fixed before release is ready (e.g., bash completion)
  - other big changes: package dependencies change, changes to engine, will be interested to see revdeps results
  - action item (shon)
- Sustainability of dev practices and releases (@shonfeder)
  - Shon's intention to step back from being official "release manager" with this (or next) release
    - Release process automations should be in place by then, and so should be a good time for hand off 
  - Also motivate by (IMO) strain as a result of missing a number of best practices
    - PRs missing descriptions or informative comments
    - Lacking traceability of defect introduction and fixes (what introduced a defect, why does a fix fix it?)
    - Fixes merged without regression tests
    - PR reviews not enforced
    - Missing change-log entries (tho just added PR template may help, can also address via opt-out CI check for changelog)
    - Should we consider a pre-release process?
      - Could give signal on problems before we cut an "official" release
      - Ali and Marek: not clear who/how it would be used, or how different than just cutting the minor release
      - Shon agrees