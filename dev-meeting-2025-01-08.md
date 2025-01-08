## Agenda

- Discussing the introduction of a `triage` label and do a regular "dig into issues during the dev meeting" (@maiste from an idea of @leonidas-from-XIV)
- Release policy for Dune and [maintenance](https://github.com/ocaml/dune/issues/11272) (@maiste)
- Binary distribution of preview, LSP and ocamlformat (@maiste) 
- Cache (@ElectreAAS)

## Meeting Notes

Attendees: @gridbugs, @ElectreAAS, @art-w, @maiste 

### Binary Distribution

- We should give control about the preview and binaries to the infrastructure team. The Dune team would be in charge of the correctness and ensuring it builds. 

### Discussing the introduction of a `triage` label and do a regular "dig into issues during the dev meeting"

- We are going to do issues digging during the Dune Dev Meeting. The goal is to decided what we should do with issues when we don't know about them.

### Release policy for Dune and maintenance

- The maintenance field is about what we want to maintain. In our case it seems to be `latest` as we don't maintain old version.
- Opam archive is to remove package from opam, but it is not directly correlated to the maintenance field.
- For the release process, we will do review quarter by quarter because we want to see if there are concrete features we can add to the new version.

### Cache 

- There is a bug in the cache.
- The PR needs reviews from people with knowledge on it. 
- Not directly in the engine, but close to it.
- With the changes, the cache doesn't seem to do the correct digest and consider some items as not cached.



