## Agenda

- `3.18` release (@Leonidas-from-XIV)

## Meeting Notes

Attendees: @rgrinberg @Leonidas-from-XIV @shym @art-w

### 3.18 (Marek)
- Release soon, only `dune subst` fixes PR to merge 
- Can also be a a minor release later
- [Issue in question](https://github.com/ocaml/dune/issues/11290)

### opam-health-check (Marek)
- Progress on the health check to build opam packages with dune pkg with CI. 
- Most issues comes from invalid dependencies in dune-project, could be fixed by using the opam files instead (as they are more up to date in opam-repository).

### Solving speed (Rudi)
- Solver should be fast enough (<1s, including the parsing of opam files)
- running it multiple times for portable lockfiles should be fine.

### Package build failures
- Some package dependencies fail to be solved because `avoid-version` are ignored (they were previously sorted into the back of the list but this caused other issues so we don't do that anymore).
- Maybe introduce a flag to enable them. 
- Solver errors are often unrelated to the real issue, hence all the accidental reports on `system-mingw`.
- Better error messages are always welcome

### Cross-compilation in Dune (Sam)
- @shym is looking intro cross-compilation. Dune pkg would need to be aware of the secondary compiler.
- The upstream compiler can cross-compile if configured with a compile-time option
- Cross-compliation is now mostly handled through findlib features

### Opam `{build}` dependencies (Rudi)
- Dune currently ignores opam `build`-flag on dependencies (which indicates that the result should be the same irrespective of the tool version, which is unsound). 
- Dune could interpret those dependencies as non-transitives, such that they are only available to packages that explicitly depend on them. 
- It would help cross-compilation, as build tools are not intended to be compiled for the targeted platform, only the host during compilation (typically preprocessing and build systems). 
- Would also potentially allow for different versions of build tools used by dependencies, however that would require the solver to be aware of it

### Which packages are failing (Marek)
- Remaining issues for dune pkg:
  * support for `or` dependencies (also in project files), 
  * avoid-version
  * in-out dependencies (lwt!)
  * resolution timeouts
  * applying patches, 
  * missing files

### Multiple nested Dunes (Rudi)
- When building packages Dune calls itself recursively
- This is slow as then it needs to create its own _build, copy sources again etc.
- Currently a limitation as the source needs to be in the source tree, but packages are fetched into the build folder
- We can't attach rules to the same folder twice, but we can can run the fetch rules in a special context and the build rules in the original, thus apply rules to the same folder without copying

### Upstreaming (Marek)
- No news at this point
- Rudi can tell us whether our planned changes will cause conflicts for now. Some improvements might be so useful that this would be acceptable