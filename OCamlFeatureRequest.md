# New OCaml features needed by Dune

For OCaml code, Dune presents high-level concept to the user using low-level features of the OCaml compiler. However for some concept the low-level features are missing.

It is important to link the high-level request which leads to a needed low-level feature because the need can be fulfilled in different ways.

## Requests

### Hide libraries from the user

For implementing non transitive deps (the user can use only the listed dependencies and not their dependencies), or hiding the interface of some modules: dune needs to hide `.cmi` files from the compiler, but still show `.cmx` files. For Dune defined libraries, Dune plays split the files in multiple directories to use `-I` to show only some. However it is not possible for non-Dune defined libraries, it is not clear that the OCaml typer will always accept to hide interface used by visible interface.

It would be more straightforward to have an OCaml option `-Ihidden` which would add a directory for lookup, but without adding type information for typing the user code.

### Faster compilation during development

A fast code/compile/test is important for development, so Dune in development mode tries to keep this loop fast. Without dune people sometimes use the bytecode compiler during development because there is no dependencies between the compilation of `.cmo` and other `.cmo`, and by default there is a dependency between the compilation of a `.cmx` and other `.cmx`. Dune uses the `-opaque` option of the native compiler to achieve the same result. However then `[@inline:always]` on a call site of a function defined in the local repository will result in `warning 55` because the inlining can't be done since the `.cmx` is opaque.

Even if it is clear that OCaml is needed to differentiate the cases of a real problem (missing `.cmx` of an installed library) and an artificial one (local library), no new local-level feature has been designed and proposed for this.

### OCamldebug

  OCamldebug could understand the mangling or at least propose possible files that contains the file name https://github.com/ocaml/dune/issues/4347 .

### Separate output

Separate warning and error output? or machine readable warning error output? (but we already have a parser for them)