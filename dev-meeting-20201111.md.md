# Present

- Arseniy Alekseyev (@aalekseyev)
- Cameron Wong (@cwong-ocaml)
- Rudi Grinberg (@rgrinberg)
- Andrey Mokhov (@snowleopard)

# PR triaging

We went through recently-opened issues and PRs, discussed and assigned some of them.

## PR#3936: nested subcommands

jdimino may be best suited to review this.

## PR#3935: Add foreign compilation modes

This PR adds support for the ability to link in different object files in bytecode and in native code.
Discussed if building in two contexts is a better solution, but we can’t install a library constructed from multiple contexts.
It's unclear if artifacts downstream (e.g. .dlls) from a such a library would have to be split split into bytecode/nativecode worlds.

## PR#3928: dune file formatting style tweaks

Andrey volunteered to review

## PR#3923: commit a sphinx-generated man page containing all dune docs.

We have some reservations about committing such a large generated
thing in the repo, but maybe it's fine.

## PR#3921: update cmdliner

This is pretty much trivial, except we have some patches on top which we need to rebase.

## PR#3920: dune RPC

Why do we need async mode? To support a use case where the editor runs continuously, but the build
is only invoked by the user manually.  For this use case, the editor (or rather, a dune helper process on 
behalf of the editor) submits requests that the build system later responds to.

The responses can be e.g. the list of errors or promotion suggestions.

## PR#9312: Avoid running a pager when running 'git diff'

Seems good.

## PR#3905: ctypes

Rudi is looking at it

## PR#3904

In 4.12, empty .a files are no longer generated, which is annoying for dune. Rudi is looking at it.

## PR#3900: add a Build.t and Fiber.t constructors to Build.

One concern is that static dependency analysis (e.g. `external_lib_deps`) may break if we use those too much, 
but it seems that this is not qualitatively new, so we should still add it. (Andrey is going to look at it)

## PR#3894: odoc: stop dune from generating docs for hidden modules

@jonludlam is looking at it

## PR#3875: make it possible to override more of C flags

When dune compiles .c files it currently passes a bunch of C flags unconditionally.
This PR makes it possible to override those flags, by making :standard expand to 
those flags.

# Issue triaging

## #3937: better UI for looking at test results.

In principle, a better UI for test results sounds great. 
In practice, this needs a lot of design work to figure out how to combine it with  dune.

## #3934: Disable production of shared libraries

Rudi is going to try to implement it

## #3930: Dune should print to stdout.

@aalekseyev volunteered to look at it.

## #3919: dune crashes if .digest-db is truncated

Sounds like this may happen e.g. because of low disk space.
Rudi will try to reproduce.

## #3917: explicit executable dependencies and cross-compilation

Listing a `foo.exe` in `deps` builds an exe from the current context, not from the host one.
This means that such rules will often break in cross-compilation, even if using `{%exe:foo.exe}` works.
This seems to be an unfortunate, but natural consequence, of the semantics of `deps` and `%exe` and we 
couldn't think of how to make this less confusing.
For now the recommendation is to avoid `dep` in favor of `%exe` where thi smatters.

## #3913, #3911 vendored libraries conflict with opam-installed ones.

Seems that the problem is that vendoring mechanism is used for package management, which is 
not its intended use case. Rudi will look at that. 

## 3910: link error because -I flags are missing

Andrey will have a look.

## #3907: .h files handling

The header file handling in Dune is not flexible enough. @aalekseyev will have a look.
