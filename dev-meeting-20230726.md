Present:
@emillon
@gridbugs
@rikusilvola
@rjbou
@rgrinberg

- current releases (@emillon)
  - 3.9.2 submitted with more platform-specific fixes
  - 3.10.0~alpha1 later in the week

- opam variables (steve)
  - taxonomy of opam variables: 
      - platform specific: `arch`, `system`, etc
      - `opam-version`
      - `jobs` / `make`: what to do about these?
  - make: dune also has this variable. it's used by the BSDs to set make=gmake.
  - jobs: won't work properly because there's going to build too many things (N dunes each taking N jobs)
  - jobs is set to 1 for now
  - job protocol is better. couple years ago JS said it wouldn't be reliable enough but we could use platform specific APIs to improve that. xref: https://github.com/ocaml/dune/pull/4331
  - there are also package variables. they're not supported for now, though there's some support in pform for it.
  - the language of filters is the same in opam for build instructions and dependencies, can we merge that in dune? no, conditional dependencies need to be more static. later the bits we're adding to actions is going to be available in dune files
  - in opam, there can be a confusion between cygwin git and windows git. if cygwin one is used there can be auth issues. how does dune deal with that? it does not, it just calls the opam lib. (we use git describe but that's no issue). see: https://github.com/ocaml/opam/issues/5617#issuecomment-1650525391