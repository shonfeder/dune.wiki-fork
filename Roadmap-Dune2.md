This document describe the roadmap for Dune 2.0.0, which is planned
for July 2019.

## Overview

Dune 2.0.0 will be the next major release of Dune.  We are bumping the
major number as we are planning a couple of breaking changes.  The
most notable one is drop the support for jbuilder projects.

However, given that a lot of packages in opam haven't migrated to
Dune, we are slightly adapting the original plan in order to make this
transition smoother: Dune 2.0.0 will no longer install a jbuilder
binary, which means that the `dune` and `jbuilder` packages will be
co-installable.

Projects with `(lang dune 1.x)` will still be fully supported by Dune
2.x.

Finally, starting with 2.0.0 we are going to experiment with
decoupling the version of OCaml required to build Dune itself and the
version of OCaml used in the current opam switch.  Dune itself already
supports such decoupling, however this is not currently used in
opam.  This point is discussed later in this document.

After Dune 2.0.0 is released, the Dune team will continue to provide
support for Dune 1.x for another two years.  By support we only mean
fixing bugs and doing bugfix releases.  No new features will be added
to Dune 1.x after Dune will be released.  In particular, Dune 1.11
will be the very last feature release of Dune 1.x.  All subsequent
releases will be Dune 1.11.y.

## Roadmap

### Step 1: polishing Dune 1.x

Since we are going to fork Dune 1.x for maintenance purposes, it is
important to leave it in a good state.  This imply polishing the
latest feature additions.

The last item on this list is the rewriting and simplifying of the
handling of the inter-module dependencies for virtual libraries.
Currently it worked by merging several dependency graphs, which is
compiled.  We are rewriting this part to have a single dependency
graphs right from the start.

We also want to finish the big refactorings in flight, so that
bugfixes are easier to backport.  The last one is the [new error
reporting API](https://github.com/ocaml/dune/pull/2190) since this one
is likely to be an issue for most bugfixes.

So the two points to wait for are:
- [ ] rewriting of depencency graphs mamangement for virtual libraries
- [ ] new error reporting API

The expected completion date is June 14th.

### Step2: release Dune 1.11

Once all this is ready we will release Dune 1.11, which will be the
last feature release of the Dune 1.x.

### Step3: release jbuilder 1.1.0

We are going to make Dune 2.0.0 and jbuilder co-installable.  However,
for about a year users have been using the jbuilder binary provided by
Dune.  The original jbuilder package is now quite old and its behavior
is likely to be slightly different from the one in Dune.

To make the transition easier, we will do one last release
jbuilder. This will be created as follow: we will simply take the
repostory at the Dune 1.11 tag, rename the package to `jbuilder` and
remove the `dune` binary from this package.  We will then cut a
release from the result.

### Step4: prepare Dune 2.0.0

At this point, the master of Dune will be dedicated to Dune 2.x. The
maintainance of Dune 1.x will happen in a branch called 1.11. We will
start this step as soon as the 1.11 release is cut, which is expected
to be around June 14th or the beginning of the next week.

In a nutshell, preparing Dune 2.0.0 involves:
- adding a new 2.0 version to the dune language
- deleting support for jbuilder
- turning a few deprecated warnings into proper errors
- some other things we wanted to change in Dune 2

There are a few comments with the word `DUNE2` in the code and a few
PRs or issues in the 2.0.0 milestones.  All these should be addressed
before this stop is finished.

The expected completion date for this step is July 19th.

### Step5: Split out dune.configurator

In order for the decoupling of OCaml versions in opam to work well, we
need to change the `dune` package to make it install only a binary.
At the moment it also installs a the `dnue.configurator` library.
This library will have to be split into a separate `dune-configurator`
package.  To allow users to rely on a version of `dune-configurator`
that is compatible with both Dune 1.x and Dune 2.x, we will need to
make `dune-configurator` rely on `(lang dune 1.x)`.  In partcular,
this means that we will need it to have its own `dune-project` file.

### Step6: release Dune 2.0.0

We will release Dune 2.0.0 in the opam reposiotry.

## Decoupling the versions of OCaml

At the moment, opam users must build and install Dune using the
compiler installed in their switch.  This has several consequences:

1. Dune itself must be able to build with all the versions of OCaml it
   can build projects against
2. It is harder to test experimental compilers given that Dune must be
   able to build with such experimental compilers in order to do any
   kind of opam testing

Point 2 has been an issue in the past, for instance for the multicore
project.  Point 1 means that if we continue like this, we will never
be able to use language features that were added since OCaml 4.02 in
Dune, which is sad.

Technically, one can build Dune using OCaml 4.07 and use the resulting
binary to build projects against OCaml 4.02.  We want to use this fact
in opam.  There are several ways this can be done, for instance we can
simply create an `ocaml407` package that will install a compiler in an
`ocaml407` sub-directory of the current opam prefix and use this to
build when inside an opam switch for a compiler that is older than
4.07.

David Allsopp has a better scheme in mind and is going to try to make
it happen before Dune 2.0.0 is released, in which case we would use
it.
