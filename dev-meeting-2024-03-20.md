Present:
@emillon
@jchavarri
@leonidas-from-xiv
@leostera
@moyodiallo
@nojb
@rgrinberg

### integration of contexts in editors (initially, vscode) (@jchavarri)
- progress on the library side
- continuation of multi-context libs: [#10222](https://github.com/ocaml/dune/issues/10222)
- questions about how to coordinate changes between dune, merlin, ocaml-lsp and vscode-ocaml so that users can choose the context for which they want to fetch the build and types information from
  - what a module means (under the cursor) is now ambiguous because it depends on the context
  - suggestions to extend LSP

### 3.15 release (@emillon)

- no known blockers, will start next week