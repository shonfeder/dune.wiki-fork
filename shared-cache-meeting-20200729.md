# Present at the meeting:

* Andrey Mokhov (@snowleopard)
* Jérémie Dimino (@jeremiedimino)
* Quentin Hocquet (@mefyl)
* Arseniy Alekseyev (@aalekseyev)

# Discussed

* Quentin looked at Async, it seems simple enough to switch from Lwt to Async. (a few days of work)
* The issues discovered so far when testing with jenga were fixed.
* There's an idea of how to make communication more efficient (only upload if the other side is missing the file, except if the file is <4kb).
* Right now it's very slow to upload (~300MB of artifacts takes minutes), probably because of the disk write buffer being too small.
* We should focus on switching to Async first before optimizing too much.
* Jane Street will open-source the Krb library so we can use it when that's done.

# Next steps:

* More testing
* Port to Async
* Work on performance