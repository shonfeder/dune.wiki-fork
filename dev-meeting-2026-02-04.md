# Agenda
- odoc3 support
- bump minimal version to 4.14 (Ali)
  - https://github.com/ocaml/dune/pull/13533

# Meeting notes

Attendees: @jonludlam @Leonidas-from-XIV @shonfeder @rgrinberg @sudha247 @punchagan @Alizter Ahukwuma Akunyili

## Odoc 3 support (@jonludlam)

- There is a PR for Odoc 3 rules
- Currently links to other packages are broken
- There is `@doc-new` but it doesn't always work as it can reference non-existing libraries
- Odoc 3 fixed a lot if these issues
  * External packages now link to ocaml.org
  * Caching has been fixed
  * Error reporting now only reports on things that the documentation author can fix
- New PR fixes `@doc` which works like the old `@doc`
- There is also `@doc-full`
  * Circular dependencies can be an issue
  * `@doc-full` needs to get all dependencies
  * `@doc-full` builds the sherlocode database
  * The sherlocode database does not have incrementality, has to be built in one go
- Link to the `odoc-config.sexp` PR, which defines which packages to depend on
- The `post` flag is important to avoid circular dependencies. Previously there was a special field in OPAM but `post` dependencies obsoleted that field
- It is crucial that Dune supports `post` dependencies
- Why shouldn't `odoc` be marked `with-doc`?
- @jonludlam likes the current semantic of just picking `depends` which have `:with-doc`, also adding `:post` manually is fine
- Conflicting rules: odoc is currently generating `odoc-config.sexp` which conflicts if Dune generates the same file
  * We can avoid generating the file if the file is going to have no meaningful configuration
- There is an issue with Angstrom which installs subpackages for Lwt & Async
  * They are separate packages but the main package still installs some bits
  * What should Angstrom actually do? Probably stop doing this
  * Could lead to breaking changes if `@doc` breaks on Angstrom
  * These failures are unlikely to be detected in OPAM CI because the OPAM CI does not build documentation
- Can we add `:with-doc` testing to the OPAM Repo CI?
  * Compute is not that expensive, Jon and Mark tried on a single machine and it took 6h for all packages
- Can a test be written to exhibit the same issue as Angstrom has, but without requiring Angstrom?
- @jonludlam is not too worried about this particular issue, more about currently unknown unknowns
- `@doc-new` can be removed. It is not used and people won't miss it if `@doc` is strictly better
- There is a PR about Markdown support, the Dune team will prioritize merging it
  * It requires odoc 3 anyway
  * It's been a long time in the making
  * Will be a painful rebase but someone will have to rebase anyway
- Caching is on by default but odoc rules are possibly buggy
  * Better to enable sandboxing. It's slower but leads to more reliable rules
- odoc plugins
  * Need to manually depend on the relevant plugins, there is no autodiscovery

## Bump OCaml minimum to 4.14 (@Alizter)

- Better do it after the 3.22 release
- @rgrinberg expects pushback from users
- @Alizter created a PR to do so
- Users can use the `secondary-compiler` to use Dune on older OCaml compilers, if we have 4.14 as secondary compiler
- Eventually we want to move to OCaml 5 only

## 3.22 release (@shonfeder)

- We should start with the release process soon
- There is an OpenBSD issue in 3.21, should we do a point release for 3.21 or better to just continue rolling with 3.22
  * Opam-Repo CI added support for OpenBSD just as we broke support for OpenBSD in 3.21
- @rgrinberg would like to make sure the trace events make 3.22
- @shonfeder would like to make sure that parametric libraries in JSOO make it for 3.22
  * However, that code hasn't been reviewed yet
  * It could be delayed
- Deadline for 3.22 in two weeks
- Creating patch releases also takes a lot of time, probably better used making 3.22