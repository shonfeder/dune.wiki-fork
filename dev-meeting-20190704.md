## Dune 1.11.0

We are working on the 1.11.0 release which we expect will be the last
feature release of the 1.x branch. I.e., new features will go in Dune
2.x only after that. Given the changes we are planning to introduce in
Dune 2.x, we expect that a certain number of users will remain at the
1.x language version.

For this reason, we want to make sure that Dune 1.11.0 is mostly
feature complete so that users staying at this version of the language
can do enough. One thing we are waiting on is the refactoring the
module dependency graph management as it because quite tedious after
the introduction of virtual libraries, with reported bugs that are
pretty hard to debug and understand. This is mostly finished and we
should be able to release Dune 1.11.0 soon.

For the record, staying at the 1.x language version means using either
the Dune 1.x or Dune 2.x binary but limiting the features used in
one's project to the ones supported by Dune 1.x, hence ensuring that
the project is compatible with both Dune 1.x and Dune 2.x. Dune makes
it easy for a users to do that by simply writing `(lang dune
<version>)` in a toplevel `dune-project` file.  For instance writing
`(lang dune 1.10)` will make Dune refuse to take advantage of features
introduced after Dune 1.10.x in this project.

## Dune 2.0.0

The aim is to make the release of Dune 2.0.0 as transparent as
possible even though we want to change quite a few things. If no opam
user notices that we released dune 2.0.0 and no constraint of the form
`"dune" < "2.0.0"` is added to the opam-repository (except for a
couple of special cases), then we will have succeeded at this task.

In particular, this will make it much easier for everybody to adopt
the Dune 2 binary. We care about users migrating to the Dune 2 binary
as this will ensure that we get reports mostly about Dune 2 rather
than about both Dune 1 and Dune 2, hence making things more manageable
for Dune developers.

### dune-configurator

We want to make Dune 2 be a pure binary distribution, so that it is
easier to package it in various package management systems. For this
reason, we need to extract the `dune.configurator` library. Its code
will remain in the dune repository, however it will now be distributed
as a separate `dune-configurator` package. The name
`dune.configurator` will continue to exists so that existing released
packages using dune can continue to build without modification.

We will do a first release of `dune-configurator` based on Dune 1.11.0
so that users can rely on this new package and remain compatible with
Dune 1.11.0 if they wish.

### Requiring a recent OCaml version to build Dune itself

At the moment, Dune is able to build with all version of OCaml
versions since 4.02.3. This fact helped Dune's adoption, but as time
goes it is putting more and more pressure on the Dune team. We need to
test against more and more OCaml versions in our CI and we cannot use
new features of the language.

In order to keep the development of Dune dynamic and keep the project
modern, we want to switch to a mode where we move freely to new
compilers as they are released. In particular, we plan to require
OCaml 4.08 to build Dune 2.0.0. By switching to this mode, When
multicore becomes available in OCaml we will be able to take advantage
of it immediately.

However, we will continue to support building OCaml projects using all
OCaml versions since 4.02.3. More precisely, it will be possible to
use a Dune 2.0.0 binary built using OCaml 4.08.0 to build OCaml
projects with OCaml 4.02.3. This is not currently supported by opam,
however we have an idea to make it work, i.e. to allow installing Dune
2.0.0 in a 4.02.3 opam switch. When the opam switch is older than the
minimum version of OCaml required to build Dune 2, it will still be
possible to install Dune 2. It will simply take longer. We will wait
for this system to be in place before releasing Dune 2.0.0.

Once nice side effect of this change is that it will make it easier to
experiment with patched compilers that cannot build Dune in opam.
This was the case recently with multicore for instance, since Dune
uses the `threads` library which hadn't been ported to multicore yet.

## duniverse

We quickly discussed the status of duniverse. Support for vendored
directories in dune is the last feature required to allow playing with
the duniverse tool. This feature is being reviewed so we should be
able to experiment with duniverse soon.

## Jbuilder compatibility

The original plan was to completely drop support for jbuilder projects
in Dune 2.0.0, in order to clean up the Dune code base, and to make
Dune 2 and jbuilder non co-installable.

A lot of projects haven't switched to Dune yet, so to avoid the
releases of Dune 2 from causing a lot of churn, we decided to make
Jbuilder and Dune co-installable since this is easy to do. This will
allow to co-install projects using Jbuilder with projects using Dune
in opam.

While this helps with opam, this is still not enough for mono-repos
and in particular for duniverse. We found a way to provide 90%
compatibility with jbuilder projects at a very cheap cost, so we are
going to do that. The idea is to simply upgrade `jbuild` on the fly as
we read them. With this method, we only need to keep the upgrader,
which is a very self contained feature that won't get in the way of
new development.

## Using Dune at Jane Street

Jane Street is moving forward with the plan to migrate from Jenga to
Dune.  Currently, JS is preparing a version of Dune that can
understand the internal Jane Street jbuild file format.  Once it can
understand enough, JS will be able to start experimenting building
parts of their big mono-repo with Dune and work torwads making the
Dune development experience be at least as good as the Jenga one. This
should benefit all Dune users as well.

This version of Dune will be simply the upstream Dune code untouched
but linked together with internal JS libraries so that it can interact
well with JS internal systems.

This method will ensure maximum sharing of all the general purpose
features expected of a good build system while still allowing to have
custom internal features that are specific to a particular group of
developers.
