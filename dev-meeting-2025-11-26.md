# Agenda

* Explicit specification of lock dirs and how they interact with the default lock dir (@Leonidas-from-XIV)
* What is the `dune pkg` workflow we want to present as default? (@Leonidas-from-XIV)
* `DUNE_CACHE_HOME` (@ElectreAAS)
  - https://github.com/ocaml/dune/pull/11612
* Dev tools configuration (@Sudha247)
  - The crux of this is captured in https://github.com/ocaml/dune/issues/12777 and https://github.com/ocaml/dune/issues/12756.
  - The solver behaviour is different from that of `opam` where it treats all repositories with same priority whearas opam has clear priority order for pulling in dependencies.
  - Also, it would be good to have a way to specify constraints for dev tools.
* Release 3.21 (@shonfeder)
  - Delayed on Shon, will be focus today
  - Reverting read-only promote first
* When are dependency updates to be allowed (@shonfeder)
  - I.e., not before invocations of commands unless local dependency specification has been updated
* `dune-warnings` error on main (related to #12766, @ElectreAAS)