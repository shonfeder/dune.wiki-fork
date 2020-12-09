# Present at the meeting:

* Andrey Mokhov (@snowleopard)
* Jeremie Dimino (@jeremie)
* Quentin Hocquet (@mefyl)

# Discussed

* Andrey worked on Jenga tweaks to make it able to access the cache.
* Andrey is still working on the PR to unify hash algorithm between Dune and Jenga.
* Quentin restored hint-mode, but it yields no improvement as the local build always beats the cache :-(
  * Will try building JS universe in -j 1, as this should definitely leverage the cache.
  * Will try building a project with fake rules that sleep.
* Quentin will check everything is easily buildable using the OPAM lockfiles.