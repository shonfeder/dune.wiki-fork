Present: @rgrinberg @gridbugs @rikusilvola @rjbou @Leonidas-from-XIV

Multiple OPAM repos
===================

* @rgrinberg talked to @dra27 about how opam will deal with repos in the future and it looks like it will be just git going forward
* We can do git-only before OPAM does, since we don't need to keep support for existing users
* We need support for multiple repos
  - How to support multiple forks of the same repo?
  - Maybe keep all repos as remotes in a single repo to avoid duplication if people have the same repo forked? Then use `git archive` to materialize the files for the solver
  - Repos set in `dune-workspace` file, with priorities
* Remove our existing HTTP support

Conditionals
============

* [@gridbugs PR](https://github.com/ocaml/dune/pull/8443) is going to yield nicer to read lock files, since it basically mirrors the OPAM filtering support so the translation is 1:1
* [@Alizter PR](https://github.com/ocaml/dune/pull/8324) is a more general solution, but the files would look rather convoluted. `case` and `cond` can also be added later

Windows support
===============

* For Unix builds we can just concatenate paths and be done, but Windows only support limited length environment variables and thus search paths
* For Windows we'd need to create simpler folders with all in one folder in the file system and add that to the search path
* @rjbou asks whether we expect to require Cygwin
  - We do require *some* Unix environment with tools like `git`, `curl`, a compiler, but it can be anything. We don't install any system tools.
  - MSVC is not supported on OCaml 5 again yet