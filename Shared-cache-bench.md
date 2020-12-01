# Methology

The current methodology is to 

1. Build the project with the cache disabled, to assess whether we impact performances when the cache is enabled.
2. Build the project with the cache enabled, local and remote cache empty. Ideally this should be identical to the previous result, safe some marginal cache checking overhead.
3. Clear the local cache and build the project with the remote cache populated. This should be significantly faster as artifacts will be fetched from the remote cache.
4. Build the project again, with the local cache still populated. This should be orders of magnitude faster as nothing should be rebuilt.

The result will always be tldr'd as three percentages which are the respective speedups of steps 2 - 3 - 4 compared to step 1. The first item should be close to 100%, the second as low as possible and the third even lower.

# Results

## Dune

Bench while building dune itself.

TLDR: 100.7% - 34.7% - 13.0%

### Versions

* date: 2020-12-01
* dune: `499a1135c61fa77d354c00abbad76950755280be`
* dune-cache-daemon: `19724e7ecff5e69409e3c4fce3378aa6ca58f97a`
* dune-distributed-storage: `bfeb5dac9e0777b9857a3c3974374ef0864ccab5`

### Results

```
$ time DUNE_CACHE=disabled dune build                     
Done: 5399/5402 (jobs: 1)
time: 14.263 real  1:14.04 user  29.846 system (728%)
$ time DUNE_CACHE=enabled dune build                             
Done: 5399/5402 (jobs: 1)
time: 14.374 real  1:13.91 user  29.339 system (718%)
$ rm -rf _build ~/.cache/dune/db/{files,meta}/v3                 
$ time DUNE_CACHE=enabled dune build            
Done: 5382/5400 (jobs: 1)
time: 4.961 real  4.694 user  3.699 system (169%)
$ rm -rf _build                                 
$ time DUNE_CACHE=enabled dune build
Done: 0/0 (jobs: 1)
time: 1.857 real  1.649 user  0.203 system (99%)
```

