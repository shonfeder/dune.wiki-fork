## Agenda

- Relocatable OCaml (@dra27)
- Communication about the experimental features. Could we add them to the CHANGELOG but mark them as experimental (@maiste)
- Synopsis for aliases (see [issue#11522](https://github.com/ocaml/dune/issues/11522)) (@maxim092001)
- Enable package management when using `--release` (see [PR#11378](https://github.com/ocaml/dune/pull/11378)) (@Leonidas-from-XIV)

## Meeting notes

@dra27 relocatable compiler works! Last week was about doing the same changes to ocamlfind and ocamlbuild to avoid any dependencies on build paths:
- Relocatable is a stronger guarantee than reproducible, since it's always reproducible independent of location (harder to implement but then no need for a set an absolute path like Nix, or to patch the binary to rewrite paths for the new location)
- Demo of the relocatable compiler! (on windows!)
- Paths to `lib` are found relative to the location of `ocamlfind` (assumes an opam-like installation with `bin/` folder and libraries accessible via `../lib/` from the binary)
- Questions about whether the `ocamlfind` binary in the right path from the `lib/` folder, and some solutions to set the OCAMLPATH as dune knows a lot about which binaries/libraries have been installed
- Maybe available in next OCaml release

Try it with:

```shell
$ opam repo add --dont-select relocatable git+https://github.com/dra27/opam-repository.git#relocatable
$ mkdir test && cd test
$ opam switch create . --repos=relocatable,default ocaml.5.3.1
$ opam install ocamlbuild ocamlfind
````

Then the `test` folder can be renamed/copied elsewhere, and this should still work:

```shell
$ ocamlopt -where
$ ocamlbuild -where
$ ocamlfind printconf
```

---

@maiste Add packagement management features in the changelog, but flagged as experimental?
- @Leonidas as an experimental section in the changelog? a bit more work for releases but doable
- Add a tag in PRs/changelog entries to flag as experimental

---

@maiste (for @maxim): How to document aliases? with which of the three options proposed? ( https://github.com/ocaml/dune/issues/11522 )
- @Leonidas additive aggregation v2 makes the most sense (the file locs are also available so we can go for this proposal)

---

@Leonidas-from-XIV: `--release` disables the package management feature, so instead you have to specify more flags (as `--release` is an alias for a bunch of flags, including `--ignore-lock-dir`). Remove `--ignore-lock-dir` from `--release`? See https://github.com/ocaml/dune/pull/11378
- Number of packages affected by this change is rather small. If there's no lock dir then package management is ignored anyway so little impact.
- 3 or 4 packages to fix: normally easy to fix by adding `-p`, but some issues with vendoring in old versions of `ocamllsp` (?)
- For FramaC either fix with `--release --ignore-lock-dir` or add the necessary `-p`

---

@Manas(prometheansacrifice) How to help dune on windows? Is it too early or will the team be receptive to PRs?
- @Leonidas: @gridbugs was working on this too, main issue was the tooling
- @Manas: What to do with cygwin? cygwin is expected by a bunch of stuff, should cygwin be bundled with dune? (the compiler doesn't work without cygwin)
- @Leonidas: cygwin should probably be part of the binary distribution yes. There's a windows roadmap by @Sudha247 and @Leostera, also questions are welcome in dune github issues
- @Manas: when compiling the compiler on windows, libraries from cygwin/mingw environment are required (it would be very ambitious to do without cygwin)
- Perhaps this can also be fixed by binary distribution of compiler and tooling?