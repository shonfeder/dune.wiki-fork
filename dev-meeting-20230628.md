Present:
@alizter
@emillon
@gridbugs
@leonidas-from-xiv
@lubegasimon
@nojb
@rgrinberg
@rikusilvola
@rjbou

- update on release process (@emillon)
  - good discussion regarding milestones
  - this is a "permanent" artifact so it makes sense to use it for something permanent (keeping track of when something has been introduced)
  - release management moved to checklist issues
- dune-build-info and vendored dirs (#8025, @emillon)
  - we agree that picking info from VCS is not correct
  - ideally we'd have something like "the version as vendored in downstream project version X"
  - could have a feature like base version prepended
  - opam-monorepo could make the bridge
  - in the meantime returning None is an improvement
- new benchmarking metrics (@alizter, #8063)
  - what kind of GC info is useful?
  - size of major heap is the most relevant one
- `arch_sixtyfour` variable
  - different behavior in CI and on Ali's computer
  - versioning: we're laxer than in the rest of dune but that's ok
  - do we need also word_size?
  - in which context are variables evaluated?
- windows regression
  - it has been introduced in 3.8.1 or .2 by a fairly large PR
  - it shouldn't have been backported
  - we don't understand how this can be triggered without watch mode
    - (just after the meeting, an investigation revealed that a queue is created and just polling can trigger race conditions)