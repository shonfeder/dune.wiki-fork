# Present at the meeting:

* Andrey Mokhov (@snowleopard)
* Jérémie Dimino (@jeremiedimino)
* Quentin Hocquet (@mefyl)
* Arseniy Alekseyev (@aalekseyev)

# Work

## Quentin

* Accept Jenga values in metadata files and ignore them for now.
* [Fix a bug](https://github.com/ocaml/dune/pull/3628) when a data
  file is missing in the cache.

## Andrey

* Release new cache format.
* Start testing Jenga + cache daemon.

# Discussions

## Avoiding multiple downloads

It appears we may often try to download the same file multiple times
concurrently if similar builds are triggered. We should avoid
downloading them twice in parallel.