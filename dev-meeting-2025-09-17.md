# Meeting notes

Attendees: @ElectreAAS, @Leonidas-from-XIV, @art-w, @rgrinberg, @Alizter, @aguluman

## Parametric libraries (@art-w)

- Good progress!
- There will be more PRs going forward
- Modules have to be instantiated in dependency order
  * For local modules that is already computed by dune
  * For OPAM dependencies ocamlobjinfo can be used, but make sure it is not called multiple times
- Executables that need to instantiate libraries might cause duplication
  * Store instantiation in a shared location, this is what js_of_ocaml is doing in some places
  * Preferrably path-addressable, but otherwise a similar hashing strategy can be used as PPXes do
- Need to fix Merlin and Odoc
  * Jane Street has some basic fixes that makes Odoc not crash
  * dot-merlin can be patched

## Copy lock dir rules (@Leonidas-from-XIV)

- How to copy from the build directory to the build directory?
  * Any way is fine as long as dependencies are tracked
- Race condition for writing to source and reading the source to determine whether generate the rules
  * The dev-tools are doing this in a broken way, locking and running is not supported in that way
  * It's ok to exclude them for now, it's an experimental feature anyway
  * Dev tool lock files should live in `_build` anyway, the fact they're in the source is a hack
- Location reporting
  * Need to map back to the original source if it was copied from the source
  * There is no general mechanism to do this mapping, it's on to just postprocess

## Portable lock file slowdown (@rgrinberg)

- Slows down parsing a lot because it checks for portable lock dirs a lot
- It is on the Tarides MVP target list but it would be good to get it fixed soon
- Will help copy rules as well
- Maybe @ElectreAAS can fix it, the fix should be fairly simple
- Otherwise @gridbugs should be back by the end of the month
- [#12244](https://github.com/ocaml/dune/issues/12244) and [#12248](https://github.com/ocaml/dune/issues/12248)