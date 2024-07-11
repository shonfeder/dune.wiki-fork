Present: @emillon @maiste @nojb @Leonidas-from-XIV @moyodiallo @ElectreAAS @lostera 

## Admin (@Leonidas-from-XIV)

- moving the meeting invite from an ICS file to a hosted calendar as that allows us to easier modify and share custody of it
- will be continuously updating the links to it once everything is properly in place
- potentially moving to a single time slot instead of the staggered but it requires input from people in other timezones

## Toolchains

- we would like to know the progress on this but @gridbugs is not here

## Distribution work (@maiste)

- idea is to get Dune from another place than just opam, thus not needing opam
- binary distribution
- currently Mac arm64, amd64; Linux arm64 static build supported
- created nightly
- switching the hosting to cloudflare to save on hosting costs
- cloudflare also supports custom URLs so we are not locked into their domain
- building the `toolchains` branch at 1 AM
- other option is to use the Github release mechanism but that would require a fork since releases can only be created from tags and we don't want to pollute the ocaml/dune repo with nightly tags
- Git LFS is not considered acceptable use by Github
- large Github repo is also not an option, Git is not good with giant repos
- Github artifacts are limited to 30 days age, deleted afterwards
- 60 MB in binaries per nightly run
- transfer costs are included as the free transfer limits are fairly generous for the amount of traffic we expect
- billing alerts exist and there is DDoS protection

## Feature flags (@ElectreAAS)

- testing things gated behind feature flags needs some way to override the disable on `main`
- @emillon notes that there is an already-existing mechanism that could be used in [dune_config](https://github.com/ocaml/dune/blob/3.16.0/src/dune_config/config.mli#L8-L10)
- a lot of tests assume the current behavior and break, need to look at them individually

## ocamlformat (@moyodiallo)

- latest update is that the dev-tools PR ow takes the ocamlformat version from `.ocamlformat`
- still needs test to show what happens if the version is invalid e.g. unavailable in opam
- is there a spec on how the appropriate version of ocamlformat is determined?
  * First `dune-project`
  * Then `.ocamlformat`
  * Otherwise latest in opam-repository
- do we even want to use `.ocamlformat`?
  * Probably not in the future, prefer `dune-project` and `:with-dev-setup`
  * We can change this later
- how to avoid pulling tools you don't use?
  * tools are only locked & build when used (in the case of `ocamlformat` the `@fmt` and `fmt` CLI)
  * other tools would be locked and built at their own entry points (e.g. `dune utop`)
  * with the dune cache enabled a lot of build artifacts of the tools could be shared to reduce the time for updates etc
- all dev-tools get locked and built separately

## Locking (@moyodiallo, @lostera)

- what is a lock?
  * it's a reproducable snapshot of the world (opam-repos) at lock time
- do we need multi-platform lock files
  * how bad is locking on one platform and using that lock on another? does that work?
  * issue is that some opam-packages are platform specific, thus the dependency cone can be different from platform to platform
  * how many packages are like this and do they need to be like this? can it be changed?
  * how does Rust/Cargo do it? Rust packages handle the platform dependent code within packages. [Platform dependencies](https://doc.rust-lang.org/cargo/reference/specifying-dependencies.html#platform-specific-dependencies) exist now in an unstable release