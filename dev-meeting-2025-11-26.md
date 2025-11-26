# Agenda

* Explicit specification of lock dirs and how they interact with the default lock dir (@Leonidas-from-XIV)
  - Rudi: should be helpful to look at how we are handling contexts
  - If we have a
* What is the `dune pkg` workflow we want to present as default? (@Leonidas-from-XIV)
  - Should we not allow `(pkg enabled)`
  - Current design of `dune pkg` is inconsistent with how contexts work
  - Rudi: we have two types of users and it is impossible to make them both
 happy
  - Concrete steps
    - Maybe introduce ways to have more convention?
    - We need a holistic design thinking about how
  - lock directory configuration
    - We have reached conclusion that we will need to specify all the systems
 one wants to lock in
  - We also agreed that we'd want a way to specify this for the default context,
 and within each context.
* `DUNE_CACHE_HOME` (@ElectreAAS)
  - https://github.com/ocaml/dune/pull/11612
  - Motivation: routine's toolchain cache was going in the wrong place
  - Want to be able to configure all dune caches (not just build) without
 breaking changes
* Dev tools configuration (@Sudha247)
  - The crux of this is captured in https://github.com/ocaml/dune/issues/12777 and https://github.com/ocaml/dune/issues/12756.
  - The solver behaviour is different from that of `opam` where it treats all repositories with same priority whearas opam has clear priority order for pulling in dependencies.
    - Rudi: we are supposed to be respecting the order of repos
      - but priority of repos only comes into affect when the same version of
 packages are available
      - but first the solver has a preference for latest version
    - Dune should look for packages in the list of repos in the order of deps
 specified
    - But do dev tools respect this?
  - Also, it would be good to have a way to specify constraints for dev tools.
  - Why can't we reuse `:with-dev-setup` for constraints?
    - We all agree this would be nice from a UI perspective, but isn't clear whether we can handle all cases reliably
* Announcing "stability" of package management
  - Rudi: 
    - We need to document the breakage with dune watch and dev tools
    - Format of lock directory is not stable
        - We do not yet guarantee that we support the lock format going forward
  - Ali is working on pref regressions
* Release 3.21 (@shonfeder)
  - Delayed on Shon, will be focus today
  - Reverting read-only promote first

