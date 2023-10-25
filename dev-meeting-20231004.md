Attendees: Rudi, Riku, Ali, Raja, Niclas, Marek

Updates
-------

Rudi: Working on improvements on how we interface with the solver

Marek: fixing the multi-repo work

Opam-repositories
-----------------

OPAM does not do any deduplication, there is just a global opam-repository. In dune-land we use a specific version of the repository per project, so that would lead to a lot of duplication.

Single version pinning
----------------------
We need the user to provide us a version for pinned packages, and then we can remove all the other candidates from the list given to the solver. This way it will force that version.

Pining a package that depends on something not in the repo
----------------------------------------------------------

Rudi will post an example in the Github issue.

dune install
------------

What is it for and why doesn't `@install` do the same? `dune install` is not supposed to be used in OPAM files, it just directly copies files into the OCaml installation but doesn't let OPAM track what was installed.

Potential improvements: Print a warning when installing into an OPAM switch. Or use OPAM instead.