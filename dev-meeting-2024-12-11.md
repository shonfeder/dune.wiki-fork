## Agenda

- Discuss In and Out problem
- Inclusion of @nojb [proposition](https://github.com/ocaml/dune/pull/11189) in `dune.3.17.1` and the rules with Dune releases (@maiste)
- Findlib dependencies -> Opam discussion (@maiste)

## Meeting Notes

Attendees: @nojb @maiste @gridbugs, @Leonidas-from-XIV, @ElectreAAS, Florian Verdand

### Inclusion of @nojob `cmxs` fix in 3.17.1 point-release

- The original behavior was wrong
- No risk in merging because the changed behavior can be fixed by fixing the `dune` file
- It's a performance issue since building is pointless and takes long
- Is it urgent?
  * We might not be able to avoid doing a 3.17.1 release anyway due to other potential regressions
  * We can include it in the next release, be it 3.18 or 3.17.1, since the effort is comparable for each

### Where should the 3.17 binary be?

- We currently release previews on preview.dune.build
- Where do the releases go?
- Eventually the plan would be to be on ocaml.org
- We can host the artefacts on the Github release, since they only need to be created per release, not daily
- For now we can do something similar as preview.dune.build does, just use the release until we figure out the ocaml.org integration

### Library name to OPAM package mapping

- Idea: using libraries in `(libraries)` should promote them to the relevant `package` section in `dune-project` automatically
- We don't know what the OPAM package for a library is named
- Proposal of a new field in the OPAM metadata, akin to `x-libraries` or `x-findlib-names` or something like this
- Dune can generate these correctly for the OPAM files it generates because Dune knows which libraries will be in which package
- Version constraints could be put in `dune` files on the libraries stanza, potentially.
- The mapping would need to be from library name to OPAM package + version, since packages might add or remove libraries over time
- Conflicts between multiple packages need to be resolved by opam-repo maintainers
  * This is already a problem because it's not possible to install all packages at the same time, so `conflicts` would need to be added for those. The problem already exists today.
- A design mistake that names can differ between OPAM and findlib
  * Especially for `_` vs `-` in names.
  * Dune could consider `_` and `-` equal in certain contexts, as it is unlikely that different OPAM packages with exists that only differ in `_` and `-`
  * `base_unix` exists, a new, unrelated package `base-unix` would probably raise some eyebrows
- We should create a document to discuss the idea with opam-repo maintainers and other interested parties to have an agreement before pushing our solution unilaterally and facing opposition

### Watch mode autolocking

- For watch mode, we can place the current versions as constraints to prevent significant and unexpected updates
  * This will create a dependency between `dune build` and the opam-repo
- The lock directory would become a lock target
- Can we have lockfiles per platform?
  * There is not such a thing as a platform, it's just a combination of OPAM variables that can have arbitrary values
  * All the lockfiles would need to be updated at the same time, to prevent out-of-date lockfiles
  * That would incur the time for the solver n times
  * We can make the solver faster, probably, but it will still take time

### 3.17 regression

- Shon and Mark have reported issues that dune needs a `dune-project` now
- We are not aware of any deliberate change about that
- It's more surprising that this ever worked, since requiring `dune-project` has happened in Dune 2.0 or so
- Steve is going to try to create a repro case
- Etienne can then try to bisect which change caused it
- We might need to undo this bit once we know why this is