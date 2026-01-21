# Agenda

- 3.22 release (Ali)
- 

## 3.22 release

- @shonfeder has finished the 3.21 release. No more regressions found in opam CI.
- 2 months of work already since the last release branch
- Anything that needs to get into the next release?
  - Rudi hasn't thought of anything. Will share updates on Zulip if anything comes to mind.

## In & Out bug

- Install rules need to be split out?
- Mismatch between which binaries/libraries a package provides. 
  - Currently, there's no way to know from where they are coming.
  - Ask for some metadata in opam for this
    - There's an issue for this
    - This needs collaboration from all non-dune packages
    - May not be the basket to put all our eggs in
  - Maybe we just depend on the package dependencies in `dune-project`. 
