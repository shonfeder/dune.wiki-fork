## Present at the Meeting:

- Arseniy Alekseyev (@aalekseyev)
- Jérémie Dimino (@jeremiedimino)
- Andrey Mokhov (@snowleopard)
- Rudi Grinberg (@rgrinberg)
- Nicolás Ojeda Bär (@nojb)
- Lubega Simon (@lubegasimon)

## Dune configuration language

Dune language has so far evolved in an ad-hoc way, and we're looking
to re-design it to make variable handling more disciplined.
@emillon is looking at Dhall among other configuration languages
to see what ideas can be used in Dune.

## Cram testing

@rgrinberg is finishing the cram testing PR. (#3601)
The cram testing tool is integrated with dune and so we
will make its behavior versioned together with the dune language.

## private/public library proposal

@rgrinberg is working on the private/public library proposal.
The idea is to allow public libaries to depend on private libraries
if they are defined defined in the same project.

## Promotion broken in master in Dune

@rgrinberg is working on it

## Readdir with extra metadata

@aalekseyev made a PR (#3675) to augment readdir with file kind information
so we can avoid extra calls to `stat`. This PR only improves unix, but @nojb
points out that Windows could stand to benefit from it even more.
@nojb volunteered to give this a shot.

## Customizations of inline test runner compilation

@lubegasimon is working on #766: to give the user more control over how
inline test runner binaries are compiled.

## Jane Street duniverse

@amokhov is working on a duniverse of Jane Street packages.
The current sub-task is to port cryptokit build system to dune.

## Distributed cache

@amokhov is integrating dune distributed cache with jenga.

## Abstract build system interface

@cwong is working on splitting the dune lib into frontend and backend
(the rule definitions and the build system engine).

## support for subdir stanza in the included dune files.

@nojb made a PR (#3676) to make it possible to use a `(subdir ...)` stanza in a dune file that's
included into another by an `(include ...)` stanza.

## Actions read stdin from /dev/null by default

Dune system should not forward its own stdin to the actions, so that
the actions don't start waiting for the user input unexpectedly,
causing dune to get stuck.

@rgrinberg wrote PR #3677 to stop doing that.
