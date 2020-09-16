# Present at the Meeting:

- Jeremy Dimino (@diml)
- Cameron Wong (@cwong)
- Lubega Simon (@lubegasimon)
- Rudi Grinberg (@rgrinberg)
- Francois Bobot (@bobot)
- Nicolás Ojeda Bär (@nojb)
- Emilio (@ejgallego)

## Watching Dune Github repository

  People receive different level of notification from the dune repository:
    - No notification
    - All notification: too much
    - Only MR and issue creation and followed one (by filtering if their login is in the body of the message)

==> @diml will propose a document for distributing more efficiently the issues and review to people.

## Work

 @rgrinberger:
   - private libraries
   - a problem with stdlib-shim
   - CI github actions

  CI github actions: some random problems (curl failing to download some files), hard to say if they are new problem.

 @cwong:
   - keep on working on the splitting engine/rules, moving things back and forth
   - cmt stuff: disabling bytecode compilation, cmt file are not built, so no menhir support. The simple solution is in fact messy. Another solution would be to change the OCaml compiler to use cmt as an intermediate file.

 @bobot:
   - pipe action
   - documentation sites feature
   - diff:
       - source file in the source tree (so clear)
       - source file is generating  (don't want to promote)
       - not in the source tree and not generated
      => Add to the heuristic not the result of a target
   - For Frama-c not blocking but dune file are generating, so need for multi-phase perhaps. @diml said that some cleanup is needed before working on that.

  @nojb:

   - Lexify converted to dune
   - but complication on copying files
   - no variable in dune language, so need to share data using files but then it is dynamic
   - More powerful language:
      * pros: very useful
      * cons: harder to have good error message
      * cons: people keep complicated code instead of adding the right feature to dune
      * to look at: https://github.com/dhall-lang/dhall-lang
 
  bundling, scoping in order to factorize for tests libraries, ppx, ... without leaking it to the exterior and without installing them
     - Having a more general `enable_if` stanza proposed by @rgrinberg ?

  @lubegasimon:

  - color option
  - flexibility to flags

  @ejgallego:

  - Try to simplify the different coq projet in order to find common way to do things.
  - For coq, missing features: scope, ...
  - Tell people use dune only for experimental until version 1.0 of coq lang.
  - They will require in the future for people to use dune in order to be in the continuous tests