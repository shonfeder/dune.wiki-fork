## Notes

Attendees: @gridbugs, Chukwuma Akunyili, @Leonidas-from-XIV, @shym
### Portable Lockfiles (@gridbugs)

- Dune got lockfiles/directories about a year ago
- Only guaranteed to be working on the current system
- Recently new experimental mechanism to make them portable by solving for different platforms at once
- Dune can show the depexts
- But only for the machine that you created the solution for
- Version 1 could add the depexts for the specific platform the solution was locked for and record it in the solution
- However depexts are mostly the same for any distribution, only the name of the package might differ between them (`pkg-config`, `pkgconf` etc)
- Solution in review: Depexts are taken from the OPAM files and just copied into the lockfile and evaluated at build time instead
- Problem: Usually OPAM variables like `os-distribution` are fixed for build commands at lock time, but not for depext commands now, which is inconsistent
- Should build commands also be allowed to be conditional on variables?
  * This would make lock files more compact as the build command does not need to be recorded for each solved platform separately
  * @rgrinberg wasn't a fan of this, @gridbugs will dig up the previous discussion
  * Maybe worth revisiting it again, now that depexts are conditional and lockfiles are meant to be portable
- Can we drop the distribution from the dependencies?
  * Depending on one OPAM package on one distribution and another one on another distribution seems very odd and possibly nobody does that
  * That would solve the issue that there's tons of Linux distributions, thus tons of solutions to be calculated (or fewer and some platforms not supported well)
  * The compiler depends on distribtion at least on Windows, to determine whether its being installed on cygwin
- Should we remove `ubuntu` and/or `alpine` from the presets?
  * Maybe, if we have mac, generic linux and some Windows distributions that could be enough?
- Does the architecture really matter for solving?
  * The compiler depends on dummy packages that contain the architecture
  * But these can also be built on other architectures, so they could be locked
- OPAM filters don't have wildcard values, undefined variables will lead to short-circuit behavior where expressions are undefined and on the top level they will be treated as `false`
  * We could implement proper wildcards in our solver, that would evaluate `*` and `not *` and `* || <expr>` correctly
  * We should however stay compatible with OPAM semantics
  * Maybe just dummy values are fine, these will always evaluate to `false`