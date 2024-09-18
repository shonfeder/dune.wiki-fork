Attendees: @gridbugs @maiste @Leonidas-from-XIV @rgrinberg @moyodiallo

## Non-local dependencies (@Leonidas-from-XIV)

- We only use the non-local dependencies to give a package name and calculate the dependency hash
- Taking all packages as dependencies is wrong but determining the right set requires evaluating the filters/running the solver
- `OpamTypes.filtered_formula` should not be exposed outside of `dune_pkg`, wrapping it in Dune types is better

## `depext` bug (@rgrinberg)

- It's an OPAM feature where plugins can define their own variables
- Unknown depext variables make the solver fail
- We can't just set them to `false` as they can be negated
- Remove any depext entirely where the filter refers to unknown variables

## Breaking change of Dune Cache (@Leonidas-from-XIV)

- Dune cache was disabled by default, so we assume most people did not use it
- Enables us to go through the rules incrementally and mark those that are safe
- `enable-user-rules` in `dune-workspace` could be a good compromise for those that can't set an environment variable
- having a configuration in `~/.config/dune` could work too

## `dune pkg add` (@maiste)

- The interviews say that people want `dune pkg add`?
- It could be done by editing `dune-project`, pretty printing it and promoting the new file
- It requires to fix pretty printing first
- Still that would not make it work for most people as they have to add the dependency to their `dune` files
- We could add the package to all `dune` files but that would make the build slower
- It can make sense in the context of the `dune` easy mode
- We could also go backwards from `dune` files which fully qualify a library (`<opam-pkg>.<meta-name>`)
- Using the auto promote mechanism to update the `dune-project` according to the `dune` files associated with a package
- How do we know which package a dependency goes to and where does it go when it is not part of a public package
- We should have non-public packages, existing projects like Yojson have dummy-opam files for that reason
- Maybe the better thing would be to have better editor integration for `dune*` files, e.g via a Dune LSP

## `fmt` issue (@moyodiallo)

- There's a race between creating lock dirs and checking for lock dirs, thus it fails at first
- Hard to say how to fix, need to look at the code to have an insight on how to solve it

## Dune tools (@gridbugs)

- `dune tools exec <toolname>` to call a tool, `exec` allows other, future verbs
- The first time installation with locking and building can take a minute, that's blocking
- Maybe a Dune LSP would be the better solution, that way the editor can request the OCaml LSP being installed via the LSP
- The connection can be handed over to the newly installed LSP
- The LSP library can be vendored into Dune, shouldn't be too hard