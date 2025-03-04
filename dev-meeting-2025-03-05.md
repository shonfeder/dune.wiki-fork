## Agenda

- Solution for `(package (name x) (deps)`? Do we want users to have it or do we support the opam file deps when no deps available?
- Dune needs ocamlc to be able to load the default context in Dune. What should we do about it because some packages don't rely on it.
   - Delay ocaml loading until necessary?
   - For package management, the toolchain requires ocaml to be there by default?