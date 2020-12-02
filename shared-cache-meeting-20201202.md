# Present at the meeting:

* Andrey Mokhov (@snowleopard)
* Arseniy Alekseyev (@aalekseyev)
* Jeremie Dimino (@jeremie)
* Quentin Hocquet (@mefyl)

# Discussed

* Switched to state RPCS to avoid encoding executable bytes in the file streams.
* Detailed benches were made on dune and JS universe.
* Quentin is reintroducing the on-the-fly rule fetching, not done yet.
* Quentin will change the benches to add an explanatory table and bench dune initialization time.
* Andrey will implement v4 of the on disk cache to match Jenga's trimmer behavior.
* Andrey will change dune digest algorithm to match Jenga's.
* Arseniy will make a PR introducing Krb.