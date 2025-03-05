## Agenda

- Solution for `(package (name x) (deps)`? Do we want users to have it or do we support the opam file deps when no deps available?
- Dune needs ocamlc to be able to load the default context in Dune. What should we do about it because some packages don't rely on it.
   - Delay ocaml loading until necessary?
   - For package management, the toolchain requires ocaml to be there by default?

## Meeting notes

Attendes: @Leonidas-from-XIV @gridbugs @maiste @art-w @jason h.

@gridbugs: dune pkg is currently using the dependencies from the dune-project files and not using the opam files. Many projects have more uptodate opam files (at least if we read them from the opam-repo). Change the default to read from opam if dune-project declares no dependencies? At least add a warning that there are no dependencies in the dune file?
  - @Leonidas: why would opam files be more uptodate? maybe because `or`, `not`, dependency constraints were not supported before? "deprecated package name" could explain empty packages?
  - @shym: would like an error if there are any difference between `dune-project` and `.opam` dependencies, to help converging on the same metadata; Warning message could be "Will be using .opam file dependencies because ..." to at least tell the user which file lists the dependencies
  - @gridbugs: makes sense to remove ambiguity on which dependencies will be used, but as a warning and not an error to avoid breaking workflows

@gridbugs: It's possible to install dune without having OCaml installed. Running `dune build` then fails because it can't find an installed version of ocaml.
- @Leonidas: dune is to eager to call `ocamlc` for its config. Can we delay? @gridbugs: it's not obvious because there's too much to wrap in Memo to defer the call to ocaml config.
- @maiste: can we enforce that ocaml is always a dependency for dune projects? then `dune pkg lock` will install a compiler and `dune build` would work
- @Leonidas & @maiste https://packages.debian.org/bookworm/ocaml-dune Debian's dune doesn't depend on ocaml 
- @Leonidas: we can't assume that ocaml was previously installed
- @gridbugs: it's almost possible to use dune without having installed ocaml before, but then there are subtle issues with running tooling where suddenly ocaml might be missing?
- @maiste: `uv` asks the user if it can install python when it's missing
- @gridbugs & @art-w: can we make up a fake ocaml config if `ocamlc` is missing? (as we expect it'll not be used) @maiste: this might be dangerous later on? it sounds bad to use fake values for `arch` etc.
- @gridbugs: is in favor of obtaining more system informations without having to depend on ocamlc, even if that may not be sufficient to fix this issue fully