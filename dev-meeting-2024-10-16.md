## Agenda

- Issues cleaning: how could we organise? (@maiste)

# Minutes

Attendees: @gridbugs @ManasJayanth @maiste @moyodiallo @ElectreAAS @rgrinberg @Leonidas-from-XIV

- organizing the backlog of issues in ocaml/dune
  - we have many issues open, several possibly overlapping
  - should we take some time to pass over all the issues and see if any can be closed?
    - Rudi has read through all the issues this at some point
    - we're all free to do work maintaining the issues
    - labeling/triaging issues can help
    - is it reasonable to ask users to try out potential fixes?
      - feel free to close items if you think you've fixed them

- dune binary dev tools
  - why do it with dune?
    - because we decided to make dune the frontend of ocaml
  - how will it work with different compiler versions?
    - could use the `available` field so that packages are only available when the correct compiler is present
  - reason did something similar, releasing a binary lsp server
  - why not do it with other packages such as the compiler
    - no reason we couldn't as long as the package is relocatable
    - can't do it with the compiler as it's not relocatable (yet!)
  - even if we distribute ocamllsp as a binary it still won't work unless the code has been compiled
    - so the compiler will also need to be installed

- which experimental features to stabalize in dune and enable by default
  - toolchains
    - it's needed so that the solver chooses a version of the compiler that works

- this issue: https://github.com/ocaml/dune/issues/11002
  - could only allow specific variable names by default until a flag is set to allow more
  - new variables are added in opam files and from the command line so there's no way to completely disallow certain variable names in dune
  - could also fix by adding a different syntax to dune for referring to non-standard variables

- solver in dune
  - tarides has a vague plan to improve error messages
  - within the zero-install solver there is an intermediate representation that represents smt (or sat?) problems as "zero install packages" and this is not helpful to us and complicates the solver.
    - it has many features that don't apply to opam packages (e.g. "commands", "role replacements", "machines")
    - might be easier to plug opam directly into the smt solver
    - could also rewrite the solver completely though this is probably overkill
    - simplifying the solver will allow us to generate errors with enough context to make them useful
    - could also speed up the solver
  - eagerly loads opam files into the formula which makes it slow
    - applies constraints after building the formula so it loads the wrong versions


- should it be an error if a package depends on a disjunction of packages, some of which don't exist?
  - allowing this could simplify some workflows involving pinning
  - but it's a bit strange to allow packages to refer to non-existat packages
  - could at least make this an error in dune and see if anyone complains
