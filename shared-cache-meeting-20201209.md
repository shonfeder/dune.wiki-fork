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

# Post-meeting thoughts

Andrey added some thoughts for the testing scenario we discussed:

If we have three sequential build jobs `A -> B -> C`, each taking one second, we won't be able to get any
speed up with the current implementation, because the hints will always be sent a little too late. For
example, we can only send a hint for job `B` after `A` has completed (once we know the hash of its output),
so fetching `B` will be racing with building `B`.

If we have three parallel jobs `A | B | C`, each taking a second, and we run the build in `-j 1` mode, then
we expect two of the hints (say, for `B` and `C`) to successfully complete before the jobs `B` and `C` start,
so we'll have a ~3x speed up.

More generally, with the current implementation hints will always be coming too late on sequential chains
of dependencies, but they have a good chance for independent jobs where we don't have enough parallelism
to build all available jobs immediately.

My expectation is that Jane Street universe should have enough parallelism to benefit from hints but judging
from the results that Quentin reported today, it's not the case.

To start getting some benefit from hints in the sequential case, we have the following options:

* Like Bazel, don't start building a job until we know for sure it's necessary (e.g. the cache
  doesn't have the results). This can be implemented in a blocking way (where we wait until the
  response from the distributed cache comes), or in a timeout-based way (where we proceed with
  the build if we haven't received the response after a certain period of time). 

* As soon as we receive results from the cache, cancel the current build job, so we don't need
  to wait for its completion unnecessarily.

* As soon as we receive the hint for `A` results, we can send the next hint for `B`, because we
  already know what the job `A` (which is currently running) is going to produce. So we can let
  it run to completion, but we can send the next hint without waiting for that completion.
  This doesn't require blocking jobs or cancelling jobs.
