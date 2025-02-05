## Agenda

- What knowledge do we people miss in the Dune "Core Team"? @maiste
- Discussing in the open? @maiste
- Adding some presentations to attract people to the meeting? @art-w

Attendees: @rgrinberg @ElectreAAS @gridbugs @maiste

## Meeting Notes

- What knowledge do we people miss in the Dune "Core Team"? @maiste
  - We'll ask more questions on the dune-dev slack channel on the ocamllabs slack

- Discussing in the open? @maiste
  - Lots of discussions about dune happen within Jane Street and Tarides but it would be more efficient to share updates on the dune-dev ocamllabs slack to give other companies and dune contributors a heads up.

- Adding some presentations to attract people to the meeting? @art-w
  - Chicken and egg problem as it's a lot of work to prepare a presentation if we don't expect many new people to show up.
  - The set of active dune developers outside of companies has decreased over the past few years; more people used to come to the meeting.
  - Could attract more devs by making it easier to contribute, e.g. by improving documentation and triaging issues
  
- Other topics
  - Rudi has been trying to speed up the solver
    - Solver is slow due to reading data from git and parsing opam files
    - Some approaches to speeding up solves is only loading packages that satisfy the user's global constraints, caching packages so they don't need to be read/parsed during each solve, and using libgit to read data from the package repo (which is a git repo)
  - Steve working on prototype of portable lockfiles
  - Should dune prefer the newest or oldest versions of packages by default during solving?
    - It would be nice but a lot of work to fix metadata errors in opam-repository
  - Should dune package management drop support for extra-files?
    - it's been removed from opam-repository but opam still supports it and some other repos may still use it, so we'll keep it for now until opam drops support altogether