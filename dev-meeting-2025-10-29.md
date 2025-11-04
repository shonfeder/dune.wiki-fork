# Agenda

- hiding lock dirs (@shonfeder)
- lock dirs as build targets (@shonfeder)
  - reverting build tool dep sharing could unblock? (Marek)
- next release (@shonfeder)

# Meeting notes

Attendees: @aguluman, @punchagan, @Alizter, @rgrinberg, @shonfeder, @ElectreAAS, @Leonidas-from-XIV

## Release management (@shonfeder)

- @shonfeder talked with @maiste on how to release Dune, should be feasible now
- @Alizter recommended to delay the release slightly to land some changes first
  * @Alizter wants to rename some fields first, to not have to support both old and new names
  * [#12519](https://github.com/ocaml/dune/pull/12519) is not backward-compatible
    - It has to be gated on dune-lang version
    - Or opt-in via feature flag
    - Or reverted
  * Thus we have 2 blockers

## Lock directories as build targets (@shonfeder)

- Do we still think it is a valuable feature
  * @rgrinberg says yes
  * Everybody agrees that it is the right thing to do
- Can it be architected in a different way?
  * We want to have the lock dirs as build targets because they unlock a lot of desireable functionality that we want to have
  * @Alizter has [a new PR](https://github.com/ocaml/dune/pull/12653) that changes fewer things and is therefore much shorter
  * Notably the solution does not change how dev-tools work
  * We can hide the dev-tools
  * If [#12648](https://github.com/ocaml/dune/pull/12648) is changed to just update dev-tools then this is acceptable for @rgrinberg

## Portable lock dirs

