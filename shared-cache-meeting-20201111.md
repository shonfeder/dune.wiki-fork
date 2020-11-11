# Present at the meeting:

* Andrey Mokhov (@snowleopard)
* Arseniy Alekseyev (@aalekseyev)
* Quentin Hocquet (@mefyl)

# Discussed

* Andrey has integrated the executable bit in Jenga hashes.
* Andrey will look into making the internal directory structure compatible between Jenga and Dune.
* Quentin benched the current state with JS universe
  * Twofold speed increase with a filled distributed cache
  * Peak VM use in the daemon is 10G, which is huge but to be expected since we load files in memory.
* Quentin started working on the new inverted RPC protocol.

# Annex

* Time to build JS universe from scratch / with distributed cache full

```
Done: 47165/47168 (jobs: 1)time: 7:39.07 real  38:28.23 user  16:13.20 system (714%)
Done: 47165/47168 (jobs: 1)time: 3:47.64 real  9:05.87 user  3:47.52 system (339%)
```