# Present at the Meeting:

- Arseniy Alekseyev (@aalekseyev)
- Cameron Wong (@cwong)
- Lubega Simon (@lubegasimon)
- Rudi Grinberg (@rgrinberg)
- Francois Bobot (@bobot)
- @voodoos

## Work

## Relocation Library (Sites, Plugins)

Francois got his work on the relocation library into a mergeable state. Since the work is quite large, it was decided to merge it as an experimental feature and continue working and reviewing this feature when it's inside master.

## Build system API

Cameron is continuing his work on splitting dune into rules and engine. It was pointed out that the current split is less than ideal and there are some modules in the engine that are too rule specific. The relocation library changes are moving more OCaml specific details into the engine. Arseniy noted that we should move artifact substitution directly into rules to avoid this problem.

## Frama-C & Dune

Francois has a WIP branch of Frama-C that builds entire in dune.

## Validating Module Names

Ulysse noted that dune accepts invalid module names as the entry modules of executables currently. Unfortunately, some users depend on this misfeature so it cannot be fixed before 3.0.

## Private Package Libraries

Rudi made further progress on the package private libraries feature. The PR just needs to hide `-I` for libraries outside of the scope of the package private library. Francois asked if this hiding is necessary when `(implicit_transitive_deps false)` is enabled. It's indeed unnecessary but `implicit_transitive_deps` isn't well supported by the compiler and we aren't sure when this problem is going to be fixed.

## Inline test flags

Lubega's PR is feature complete and should be merged shortly.