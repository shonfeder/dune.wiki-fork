# Agenda

- the morning meeting (@shonfeder)
- pending release (@shonfeder)

# Meeting notes

Attendees: @Alizter, @rgrinberg, @shonfeder, @ElectreAAS, @Leonidas-from-XIV

- the morning meeting
  - Should we keep having it?
- pending release (@shonfeder)
  - Get parametric lib instantiation merged
  - Get the read only promoted files change fixed or reverted
  - Shon will plan to start cutting next week
- paramteric libs (@rgrinberg)
  - needs to be scope sensitive
  - important to not 
  - needs to preserve caching
  - Arthur: for inline tests, need to know for a file whether it has been preprocessed
     - b/c filenames get the `pp` infix
     - modules give filename, but the filename may not be correct, in case the files where preprocessed
     - so we cannot reliably look up a filename from a module without knowing if it has been preprocessed
     - Any more elegant way to solve this?
       - Rudi: maybe directories? different naming schemes? -- is just a pain to deal with now
- Portable lock dirs (@rgrinberg)
  - Portable lock dirs as feature should disappear as flag in code base
  - Instead just refer to list of platforms
  - Only outstanding question is what configurations do we solve for if user has not provided any information
  - What is the point of lock dir portability?
     - Ali: that people do not have to think about the locking at all
     - Rudi: not having to think about it may just not be an option
     - Shon: The important property is just that you *can* build the same locked project on different machines, even if you need to relock
     - Ali: What if a maintainer doesn't want to incorporate lock info from a new platform of some user?
        - Agreed we should have a way way to lock that appends the new platform solution, and a way to lock that only extends the platform lock locally
  - Rudi: has talked with Ryan Gibb, who has implemented an alternative solver for opam
    - based on pubgrub: https://github.com/pubgrub-rs/pubgrub (there's an ocaml implementation)
    - talks about how to solve problem of not being able to specify constraints such as a single version of package for multiple configurations
    - used in uv
    - if it is good, maybe we could swap this out for opam
  - Arthur: we could already have this behavior in the current solver if we refactor to build a single SAT problem
  - Shon: Renewing question of whether having a requirement that a single version is the right default
    - Concern about over-constraint contributing to dependency hell
    - But team confirmed that lock files are not factored into dependencies
- perf of null builds with and without `dune pkg` (@shonfede)
  - Could this only be an issue with portable lock dirs?
  - Ali:
    - there is a conditional in parsing
    - A second (minro) perf regression between released versions (dune to modular hashing)
    - what is responsible for the with/without?
       - thought maybe it could be digests? -- but Rudi said we should not be digesting on null builds
  - parsing, digesting, stating
  - last resort is make some assumptions about only dune writing into the _build and lock dirs
- eager watch mode not handling RPCs (@alizter)
  - [#12715](https://github.com/ocaml/dune/issues/12715)
  - yep, it's broken
  - Rudi: one of first things I did when joining internal dune team at JS was rewrite the RPC system
    - One of things this does is remove distinction between eager/lazy
    - The JS RPC system would not be a drop-in replacement, but could help
    - However, it won't solve all the problems anyhow
  - What about always having a build server?
    - E.g., in bazel, first launch always starts a build server if not running
    - Potential complications with opam installs