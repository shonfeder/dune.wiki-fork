# Present at the Meeting:

- Andrey Mokhov (@snowleopard)
- Anil Madhavapeddy (@avsm)
- Arseniy Alekseyev (@aalekseyev)
- Cameron Wong (@cwong)
- Lubega Simon (@lubegasimon)
- Rudi Grinberg (@rgrinberg)

# Work

## Dune Universe

Andrey made a PR to move cryptokit to dune.
xleroy asks to make improvements (a use of dune_configurator) and needs
guidance.

## Build system API

Cameron is continuing his work on splitting dune into rules and engine.

## Customizations of inline test runner compilation

Lubega keeps working on this. Expected to have something finished this week.

## Github ci

Rudi is completing a switch to github CI instead of travis and appveyor.
Should land this week. As a result:

- CI configuration is simplified
- We run tests on MacOS in addition to Linux
- We check formatting check

The new CI takes around 30min to run.

## Changing temp dir in actions

Dune uses a temp dir in the workspace now instead of /tmp.

## Package-private libraries

The work is ongoing, and the PR needs a bit more work:
cmi hiding not implemented yet.

Rudi talked to findlib devs and they don't want to spend time on the
findlib-side support for such libraries, but the dune-side things
look good.

In the future we are considering lifting the restriction that package must be
globally unique (make them scoped), so that we can use multiple versions
of the same package in the same build.