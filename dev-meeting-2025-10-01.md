# New Link

https://meet.google.com/bmh-mwbw-aqo

# Agenda

* Cmdliner 2.0 (@shonfeder, @ElectreAAS)
* Action hashing via @gridbugs new hasher interface? (@Leonidas-from-XIV)
* Clarifications around RPC situation in non-watch scenarios (@ElectreAAS)
* Branch protections for CI (@shonfeder, @Leonidas-from-XIV)
* Someone in Tarides' should probably have `maintain` perms (@shonfeder)

# Meeting notes


* Cmdliner 2.0 (@shonfeder, @ElectreAAS)
  * https://github.com/ocaml/dune/issues/12515
  * Any considerations?
    * Ali: most/any modifications in our vendored cmdliner
    * Tiny derisking subtask, whether it needs any additional work beyond the update
    * @ElectreAAS will do the initial derisking effort
    * If complex work, let's triage to determine priority
* Action hashing via @gridbugs new hasher interface? (@Leonidas-from-XIV)
  * Rudi: worth investigating
  * Upsides?
    * code hi
    * @Leonidas-from-XIV: potential perf improvement (for null builds only)
    * @rgrinberg: needs to change reused actions, like running a file etc.
  * Issue to track: TODO
* Clarifications around RPC situation in non-watch scenarios (@ElectreAAS)
  * Struggling with getting test server to work in watch mode
  * What should happen when normal dune build is running (not using watch mode) and second dune command is run
    * Conclusion: Generally should not be allowed?
  * Some RPC commands can be handled when run not in watch mode on JS internal dune
    * Ask it for e.g. progress and sets of errors
  *  `go_with_rpc` function that doesn't actually start server?
    * @rgrinberg: at some point we wanted to run RPC server without watch mode in some limited situations, so made it lazy
      * but we then realized that this laziness is not useful.
    * Conclusion:
      * if we start the RPC server in all conditions then shut it down, then we don't need the lazy starting
      * but we still need "ensure ready"
* Branch protections for CI (@shonfeder, @Leonidas-from-XIV)
  * @rgrinberg: Recent breakage is because of test dependency being updated
  * @Leonidas-from-XIV: reducing breakage on main is on motive, but allowing merge when ready is also not important
  * relation to merge queue?
     * This may be 
  * Conclusion: There are no objections, and we will proceed
  * Which CI should be required?
     * Let's start with enabling all of them, and we can selectively disable ones that prove flaky
  * Followup: whomever in Tarides is granted merge perms can configure this
* Someone in Tarides' should probably have `maintain` perms (@shonfeder)
  * Whoever will be backup release coordinator in Tarides should get the maintenance perms

Attendees: @rgrinberg, @Leonidas-from-XIV, @shonfeder, @art-w, @ElectreAAS, @Alizter, @Sudha247