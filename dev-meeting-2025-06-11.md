## Agenda

- release 3.19.1 (@Alizter)
  - needs https://github.com/ocaml/dune/pull/11879
  - any other fixes?

- time to deprecate the monorepo benchmark (Steve, who won't be present but please discuss this anyway ;) )
  - nobody is maintaining it, Steve is the only one who knows how to maintain it (though there are docs on the Tarides internal wiki)
  - originally introduced so we could benchmark dune on a large monorepo so Jane Street could get a sense for if it was fast enough for them to adopt
  - this adoption has now taken place (afaik)
  - ostensibly the benchmark is also there to detect regressions in upstream dune, but Jane Street seems to have forked dune in the meantime for the purposes of oxcaml

- OxCaml extension and pform (@maiste)
  - For OxCaml, I tried to add an extension to guard the variable `%{oxcaml_supported}` but it creates a dependency loop with `Pform`
  - Two options:
    - Manually register the `oxcaml` with a `ref` (Found it ugly)
    - Instead of a variable create a new field `with_oxcaml_supported`.
