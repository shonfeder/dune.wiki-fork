This document is a work in progress.

Present at the meeting:

- Andrey Mokhov (@snowleopard)
- Anil Madhavapeddy (@avsm)
- Emilio Jesús Gallego Arias (@ejgallego)
- François Bobot (@bobot)
- Jérémie Dimino (@jeremiedimino)
- Rudi Grinberg (@rgrinberg)

### Management of repositories under ocaml-dune

We started splitting out a few parts of parts of dune into external
repositories. Moving forward, the main repository will only contain
the `dune` binary and libraries such as `dune-configurator` or
`dune-build-info` will be provided by repositories under the
[ocaml-dune organisation](https://github.com/ocaml-dune).

Libraries such as `dune-build-info` or `dune-configurator` are part of
the libraries one uses while building their software and for instance
might end up vendored. For this reason, it is important to have some
flexibility between `dune` and these libraries. For instance, a recent
`dune` should work with old versions of `dune-configurator`.

This currently doesn't work well because these libraries make
assumptions over the private elements of Dune. These assumptions comes
both from the runtime communication between the `dune` binary and the
library or from the fact that these libraries relies on private `dune`
libraries that are not properly versioned.

This split will make each component more independant and will force us
to make the relationship between the various components explicit,
minimal and versioned.

### cmt files problem

There are various issues related to the way cmt files are produced:

- https://github.com/ocaml/dune/issues/3182
- https://github.com/ocaml/dune/issues/3467

### François sites and relocation PR

### OCaml and Rust: Cargo and Dune Integration

- Get Cargo rules into Dune so that it can invoke `cargo build` in a monorepo and know where the output artefacts are.
- Discussion thread: https://discuss.ocaml.org/t/cargo-opam-packaging-of-a-rust-ocaml-project/5743/
- https://github.com/zshipko/ocaml-rust-starter
