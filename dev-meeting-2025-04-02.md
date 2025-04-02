## Agenda

- Getting `dune exec` to update `argv.(0)` in a sensible way. (@Alizter) (5 min)
  - https://github.com/ocaml/dune/pull/11559
  - addresses https://github.com/ocaml/dune/issues/11527

- Quality of life improvements for tests (@Alizter) (5 min)
  - Aliases for inline tests https://github.com/ocaml/dune/pull/11109
  - Aliases for `(tests)` stanzas https://github.com/ocaml/dune/pull/11558

- Fixed double-running actions. (@Alizter) (5-10 min)
  - cram tests being run twice https://github.com/ocaml/dune/pull/11547
  - user actions being run twice https://github.com/ocaml/dune/pull/11557

- `dune exec -w` leaving orphan processes. (@Alizter) (10 min)
  - https://github.com/ocaml/dune/issues/11089
  - Reproduction https://github.com/ocaml/dune/pull/11562

- Documentation on tests is messy and hard to find. (@Alizter) (10-15 min)
  - We should split the how-to and the reference
  - Many issues due to people unable to read the poor documentation

- Watch-mode with auto locking. (@maiste)

