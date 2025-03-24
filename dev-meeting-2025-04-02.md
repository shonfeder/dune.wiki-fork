## Agenda

- Getting `dune exec` to update `argv.(0)` in a sensible way. (@Alizter)
  - https://github.com/ocaml/dune/pull/11559
  - addresses https://github.com/ocaml/dune/issues/11527

- Fixed double-running actions. (@Alizter)
  - cram tests being run twice https://github.com/ocaml/dune/pull/11547
  - user actions being run twice https://github.com/ocaml/dune/pull/11557

- Quality of life improvements for tests (@Alizter)
  - Aliases for inline tests https://github.com/ocaml/dune/pull/11109
  - Aliases for `(tests)` stanzas https://github.com/ocaml/dune/pull/11558

- Documentation on tests is messy and hard to find. (@Alizter)
  - We should split the how-to and the reference
  - Many issues due to people unable to read the poor documentation

