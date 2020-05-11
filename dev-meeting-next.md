## Proposed discussion topics

### Management of repositories under ocaml-dune

We started splitting out a few parts of parts of dune into external
repositories, how should these be managed moving forward?

Should we split stdune? Should we copy&paste code between the
repositories?

### Building the OCaml compiler with Dune

Let's talk about the Dune build of the OCaml compiler.  Several
compiler developers are using Dune to work on the compiler because it
gives them a better development experience.  However, the Dune build
is not officlally supported and requires after the fact patches.
