# Present at the meeting:

* Andrey Mokhov (@snowleopard)
* Jérémie Dimino (@jeremiedimino)
* Quentin Hocquet (@mefyl)

# Work

## Quentin

* Added configuration file support to shard artifacts to multiple
  backend.

## Andrei

* Working on talking to the cache daemon in Jenga.

# Discussion

## Not distributing fast rules

We might want to not distribute rules that are quick to run, as
rebuilding would beat downloading from the shared cache. That would
probably require to add a "runtime" field in the metadata files. We still
want to cache them locally because of deduplication.

## Jenga integration

Required step to support Jenga integration on the daemon side, prioritized :

* Support Unix socket.
* New "promoted" message, to inform the daemon that a rule was
  promoted directly by Jenga and some action must be taken -
  eg. upload to the distributed cache.
* Support Jenga's value directory.
* Trimming.
* Parse config file on the distributed server side, to reject and trim
  files we are not supposed to store.
* Work on Etags to not upload or download duplicate files.
