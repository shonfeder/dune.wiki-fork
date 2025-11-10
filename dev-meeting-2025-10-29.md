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

- Do we still think it is the correct design?
  * @rgrinberg says yes: when we can afford it, it is always good to make things build targets, because when you can enable it, you lose nothing and gain flexibility 
  * Everybody agrees that it is the right thing to do
- Can it be architected in a different way?
  * We want to have the lock dirs as build targets because they unlock a lot of desireable functionality that we want to have
  * @Alizter has [a new PR](https://github.com/ocaml/dune/pull/12653) that changes fewer things and is therefore much shorter
  * Notably the solution does not change how dev-tools work
  * We can hide the dev-tools
  * If [#12648](https://github.com/ocaml/dune/pull/12648) is changed to just update dev-tools then this is acceptable for @rgrinberg

## Portable lock dirs

- How should they work when different platforms are involved, e.g. for solving and running?
- The locked package versions can diverge between lock dirs
  * @rgrinberg considers this a very undesireable feature
  * This can be benign, but if different platforms get different versions then the API can also change, yielding incompatible results
  * @shonfeder: Why do we want common versions between platforms?
  * Opam is probably working on something like this in their solver
  * Common versions reduce the size of the solution space
- Unifying package versions between platform still needs to be done
- What is a reasonable default set of platforms to lock for?
  * Maybe there should not be any default set. This solves the issue that lock results are untested, the user has to opt-in to every platform, thus making them aware of the support burden explicitly
- Is that even a real-world problem? Are we sure we're solving a practical problem that users have and not just a theoretical problem that requires an unlikely setup to trigger?
- Could we require a successful build for every locked platform?
  * This is complicated as you can only build your own platform
  * Thus lockfiles would diverge over time, as only selected platforms are updated over time
- @rgrinberg is in favor of enabling portable lock files by default and abolishing the feature flag once UI fixes are in place
- We should talk to @gridbugs about it

## Hiding lock dirs (@shonfeder)

  - Marek: we should hide the dev tools lock directory?
  - Rudi: Agreed, this should be hidden
