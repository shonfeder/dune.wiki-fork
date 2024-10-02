Attendees: @moyodiallo @maiste @Leonidas-from-XIV @rgrinberg @ElectreAAS, @lubegasimon

## Where should dev-tools lock files be created? (@moyodiallo)

- Reported as [#10955](https://github.com/ocaml/dune/issues/10955)
- Review fatigue if we generate lots of unimportant info into the users repository
- Don't bother users with unimportant stuff
- Should be moved out of the source repo, e.g. into `_build`

## In-and-out-of-workspace dependency bug (@ElectreAAS)

- To be able to use `dune pkg` with dune we need to fix [#10855](https://github.com/ocaml/dune/issues/10855)
- What's the most sensible thing to do?
- Can we ignore in-workspace packages for package management? Not really, that's not consistent with the rest how Dune works
- Problem is that rules are loaded globally and with package management this can lead to dependency cycles
- We need a way to only load relevant rules
- The package handling could be moved to use a mechanism like external packages use, with cookie/install files that trigger their rules if depended on
- [#8652](https://github.com/ocaml/dune/issues/8652) exists, outlining the issues
- Could be a huge improvement to the filtering performance as unneeded rules don't need to be computed
- Could also avoid recursive `dune` calls as the rules could be loaded into the parent dune instead
- Non-trivial refactoring, multiple engineers required
- Possibly needs changing project structures to separate packages into folder or adding configuration in `dune-project` to match directories with packages
- Where do rules go that aren't attached to packages?
- Use `-p` as inspiration for guessing where what goes

## Dune release cycle (@maiste)

- Are the releases triggered in some way?
- Mostly time based releases
- Aside from @emillon @gridbugs and @Leonidas-from-XIV have released Dune before
- Long time no release, we should probably create one
- `-H` needs to be also passed to Merlin in the config, there is an in-progress PR by Ulysse, it would be good to ship these in the same release
- Need to let OPAM CI team know before creating a release to not break the CI due to large amount of revdeps

## [preview.dune.build](https://preview.dune.build) (@maiste)

- Site is online
- Looks a bit more legitimate with `dune.build` as domain