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

### cmt files problem

Producing cmt files sometimes requires more work than necessary. What
should we do about this?

- https://github.com/ocaml/dune/issues/3182
- https://github.com/ocaml/dune/issues/3467

### François sites and relocation PR

### OCaml and Rust: Cargo and Dune Integration

- Get Cargo rules into Dune so that it can invoke `cargo build` in a monorepo and know where the output artefacts are.
- Discussion thread: https://discuss.ocaml.org/t/cargo-opam-packaging-of-a-rust-ocaml-project/5743/
- https://github.com/zshipko/ocaml-rust-starter