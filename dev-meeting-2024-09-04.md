Attendees: @maiste @Leonidas-from-XIV @moyodiallo @lostera

## Agenda

- Discussion on `package` or `pkg` commands. How should we handle it.
- Discuss assessing some Terminologies: `dune-projects`, `dev-tool`, `dev-tools`.
- Auto re-locking: reminder about the blockers and if there are some possible solutions.

## Dev Team Calls (@leostera)

- We should make sure that everyone is onboard wrt to the proper time of the call
- The old invite (with the old time) has been deleted now, to avoid confusion

## `pkg` vs `package` (@maiste/@leostera)

- We shouldn't choose a prefix because we don't yet know how people are going to use this. Might not even be a prefix, e.g. `dune lock` or `dune add` etc.
- `Cmdliner` has a way of marking options as deprecated so we can remove them later in a kind of nice way
- Q: Will `dune pkg lock` disappear?
  * No, it's just that people in the DX interview tend to use `package`
  * We might create `dune lock`
- Q: If we have autolocking, will we then remove `dune pkg lock`?
  * No, there's still cases where manually locking could be preferred
- Q: Will we have autolocking?
  * It's a technical challenge to make locking work

## Autolocking (@maiste)

- Current solver input is terrible
- Hard to fix because it is a generic SAT solver which doesn't know what the context is of what it is solving, it's used for 0install and hacked to work with Opam packages
- Q: Can we get better output?
  * The solver has a way to get the diagnostics in a structured way
  * `opam-monorepo` does this, but it is hard to interpret the diagnostics
- Locking as part of `build` is a technical issue
 * It is not a build target and doesn't use the engine
 * Q: Can we make it a build target?
  - Possibly, but at the moment the engine only writes to `_build`
  - We can `promote` the results back into the source tree
- Other ways autolocking is done?
  * How does `yarn` solve this?
    - `yarn install`/`yarn add`
  * How does `cargo watch` solve this?
  * Is there a Go equivalent of this?
    - No watch mode that we know of, at least no official one
  * How does Elixir solve this?
    - A bit different because in Elixir/Erlang you keep the image running, so the overall workflow is pretty different.
- If we want to convert locking into a build target we need to make sure that this is the main blocker

## Dev Tool Terminology (@moyodiallo)

- `dev-dependencies` vs `with-dev-setup` vs `dev-tools` vs `dev tools`
- Should we standardize the terminology?
- Might not be very confusing
- We could avoid introducing another term (dev tools)
- Do we need `dev-dependencies`? What other ecosystems put into `dev-dependencies` we mostly have in `test`

## Topics to discuss next (@lostera)

- `dune add` command
- Github package dependency syntax

## OCaml.org (@leostera)

- We'll be working on making the page with the developer preview binaries nicer
- Using the ocaml.org codebase so the change can be upstreamed and we don't need to do the work twice in two different approaches
- We need to eventually use the OCurrent pipeline correctly, to not have to spend a long time at the very end blocked on writing that when we just want to release