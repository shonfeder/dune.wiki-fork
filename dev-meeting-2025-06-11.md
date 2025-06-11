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

## Meeting notes

Attendees: @maiste @rgrinberg @shym @Alizter @rikusilvola @aguluman @Leonidas-from-XIV

### Release 3.19.1 (@Alizter)

- Ali had worked on improving the way programs are launched, but it had lead to a regression
- Reverted back to the old way
- It was released earlier this morning to opam-repository
- CI was stuck because it could only use released versions, @maiste has created two PRs to enable CI to use the unreleased dev-version
  * https://github.com/ocaml/dune/pull/11884
  * https://github.com/ocaml/dune/pull/11883
- We should otherwise stick to our usual release cadence
- There are some issues setting up a clean switch
  * But it seems developing on OCaml 5.3.0 works perfectly fine aside from some warnings about `alert` in the bootstrap process

### Obsolete monorepo benchmark (@gridbugs)

- Only maintained by @gridbugs
- Made for adoption within Jane Street
- Jane Street does not need it any more
- It takes a long time to build
- It can be removed
- This is the one benchmark which takes a really long time, the other benchmarks are still useful

### OxCaml support flag (@maiste)

- There should be a possibility to determine whether building oxcaml is suported
- It can be a pform but should be guarded behind the oxcaml extension
- But determining whether to support it or not causes a circular dependency
- `set_decoding_environment` can be used to register extensions
- We have had this situation before, Melange might be doing something similar, Coq definitely does, the appraoch can be copied
- It's stored in an universal map, shouldn't cause any circular dependencies