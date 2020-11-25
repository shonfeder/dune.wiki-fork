# Present at the meeting:

* Andrey Mokhov (@snowleopard)
* Arseniy Alekseyev (@aalekseyev)
* Jeremie Dimino (@jeremie)
* Quentin Hocquet (@mefyl)

# Discussed

* The inverted protocol has been implemented with pipe RPCs.
* Andrey didn't have time yet to set things up on hydra machines.
* Currently we use the first byte of pipe RPC to encode the executable byte. Quentin will have a look at state RPCs to replace this.
* Quentin will bench again with JS Universe.
* Quentin will try to re-enable on the fly hints to fetch artifacts as we discover they need building.