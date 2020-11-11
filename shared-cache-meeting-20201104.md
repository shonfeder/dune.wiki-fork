# Present at the meeting:

* Arseniy Alekseyev (@aalekseyev)
* Jérémie Dimino (@jeremiedimino)
* Quentin Hocquet (@mefyl)

# Discussed

* Andrey proposed a new solution where the distributed server ask the cache daemon for the individual artifacts.
* This approach will enable the use of Pipe RPC to avoid loading files in memory.
* Quentin will perform benches on the js universe, including peak memory usage.
* We may have to imitate Jenga to make things more reproducible, such as changing the ar command in the PATH. 