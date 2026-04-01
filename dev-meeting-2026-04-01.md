# Agenda

- Post-action dependency refinement (Ali)
  - We (Tarides) want to make changes to the engine to allow for rules to optionally refine their dependencies
  - The refined dependencies will then be included in the target hash
  - Doing so allows rules to refine their dependencies and choose how their targets become invalidated
  - This will be an engine change which will be used by guarded experimental rules
    - One based on ocamlobjinfo processing
    - Maybe others

- When encountering `(sandbox always)` why do we choose `copy` and `symlink` over `hardlink`? (Ali)
  - Should we be preferring hardlink all the time?
  - Question came up adding hardlinking sandboxes to Windows https://github.com/ocaml/dune/pull/13987
 
- Formatting multi-line strings (Ali)
  - https://github.com/ocaml/dune/pull/13758
  - is this something we want, should we have a look at it?