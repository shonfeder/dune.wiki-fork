Present:
@emillon
@gridbugs
@jchavarri
@leonidas-from-xiv
@moyodiallo
@rgrinberg

### per-context library namespaces (@jchavarri)
- refs: [#10170](https://github.com/ocaml/dune/pull/10170), [#10179](https://github.com/ocaml/dune/pull/10179)
- Javier did a presentation of what he's trying to achieve (cf linked PRs).
- Design makes sense but this needs work to delay some checks.
- Javier ill post a description of the problem as a new issue

### Recursive aliases in vendored dirs (@rgrinberg)
- ref: [#10144](https://github.com/ocaml/dune/issues/10144)
- There are actually 2 problems:
  - projects that rely on recursive aliases in their rules, will break when being vendored (hasn't been reported)
  - recursive aliases do not propagate down to vendored dirs
- The current vendoring model is a bit weird. Doing it per directory rather than per dune-project is strange but had its benefits.
- for the second issue, whether we want to recurse or not depends on the (builtin) alias: fmt, no, check, probably.

### OCAMLRUNPARAM=b (@leonidas-from-xiv)
- ref: #10193
- deoptimizing in the dev profile is already a thing
  - (this is done in particular for jsoo, but this might not be the example to follow since it can be surprising)
- 2 different discussions: whether we should do it, and how to do it
- if the compiler had a flag to build an executable that starts with backtraces, we'd use it
- just in `dune exec` and related seem fine.
- using a wrapping script would work on unix, but not on Windows
  - is there a way to set automatic variables to `.exe` files on Windows?
- does `OCAMLPARAM` support something like that?
  - no, it's just for passing arguments to the compiler.