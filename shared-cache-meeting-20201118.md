# Present at the meeting:

* Andrey Mokhov (@snowleopard)
* Arseniy Alekseyev (@aalekseyev)
* Quentin Hocquet (@mefyl)

# Discussed

* Quentin implemented the new RPC inverted logic where the store ask the cache-daemons for artifacts when it receives metadata.
* We have a problem : artifacts may need to go to another backend server than the one that received the matadata.
  * Quentin suggests we may want to support pipes in the query of RPCs to avoid this altogether.
  * Andrey suggests we may send the metadata to all relevant backends instead.
  * Arseniy will ask Async.Rpc maintainers whether pipes in query is feasible, otherwise we'll go with the workaround. 
