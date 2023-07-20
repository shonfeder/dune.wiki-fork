Present:
@alizter
@emillon
@gridbugs
@leonidas-from-xiv
@lubegasimon
@rjbou

- benchmark bot (@emillon)
  - Comment has been deactivated

- dune monitor command (#8152, @alizter)
  - Demo
  - Can editor integration use this?
  - Bug with active job count
  - Future of the command: goal is to integrate into `dune build`

- changelog creator (@alizter)
  - Demo
  - This contains several aspects without various buy-in levels:
    - New workflow: we agree it's valuable
    - Changelog merge step: seems interesting too
    - Changelog creation step: this steps seems pretty personal and hard to standardize on for now
  - Should we output a single CHANGES.md or one per version? General consensus is that keeping a single file is better.

- case/pkg (@alizter)
  - Demo
  - New `case` and `cond` forms in action language that are evaluated at parse time. So they are not part of the action language.
  - Seems interesting in the context of package management but also works on an old proposal to bring conditionals. (why was this abandoned?)

- 3.10 release (@emillon)
  - release process starts next week
  - in case of known regressions, make sure to file issues and ping @emillon.