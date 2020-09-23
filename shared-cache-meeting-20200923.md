# Present at the meeting:

* Andrey Mokhov (@snowleopard)
* Jérémie Dimino (@jeremiedimino)
* Quentin Hocquet (@mefyl)

# Discussed

* Andrey and Quentin get similar performance results in their respective tests.
* Upload and download is way too slow, which will most likely be improved with HTTP Keep-alive. 
  * Using keep-alive with Cohttp produces invalid HTTP request, which Quentin is looking into. 
  * There might be some bugs on the httpaf side too - investigation in progress.
* Andrey had artifacts corruption on download, which Quentin had not on js/universe, a vary large repository. 
  * It could be because Quentin is using the new Async branch, which should be merged ASAP.
  * Andrey suggested it might be a race condition. However, everything is atomic so  shouldn’t be happening.
  * Quentin will look how to add a lock file so that we use exactly the same versions of the deps.
* No progress on Krb yet as the focus is on performances.

# Next steps

* Quentin
  * Merge the Async branch.
  * Provide a lock file of the dependencies.
  * Fix Cohttp (and potentiall httpaf) to make connection keep-alive work and recheck performances.
* Andrey
  * Test again with the Async branch merged and the lock file