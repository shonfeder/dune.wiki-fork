Present:
@alizter
@emillon
@jchavarri
@leonidas-from-xiv
@moyodiallo
@rgrinberg
@voodoos

## merlin indexing (@voodoos)

- presentation
- how can we make this opt in?
- actually: instead of making it part of Dune, can we make it part of just Merlin?
- discussion of the differences between the initial version deployed at Jane Street and this one
- @voodoos to try the alternative approach

## 3.15.1 release (@emillon)

- 3.15.1 with pending fixes is released and waiting in opam-repo-ci

## interpreting relative mandir paths (@moyodiallo)

- https://github.com/ocaml/dune/pull/10240
- proposition to interpret paths are relative depending on destdir
- actually, do we even need to fix this?
- need more info from the reporter (how pkgsrc passes paths, etc)

## coqdep regression (@alizter)

- https://github.com/ocaml/dune/issues/10149
- change in engine causing a regression in coq rules
- Rudi to investigate

## tiered support (@emillon)

- start of an effort to write an explicit policy regarding what we support (systems, 32-bit, versions, etc)
- goal is to use the same across projects