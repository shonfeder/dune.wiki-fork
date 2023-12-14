Present:
@emillon
@gridbugs
@leonidas-from-xiv
@moyodiallo

## lockdir compatibility (@gridbugs)
- how do we determine whether a given lockdir is compatible with the current system?
- existing systems:
  - opam (lock, switch export): no particular checks but the solver is always called so it will fail
  - opam-monorepo: no checks, it will just unpack the packages and dune might fail at build time
- dune dumps the variables that were evaluated during locking into the lockdir
- these assumptions can be checked when using the lockfile

## making ocamlfind relocatable (@emillon)
- ocamlfind captures paths at configure time and uses them at runtime
- need to make this relocatable
- we also need to ensure that for "system" installations this continues to work

## JS features upstreamed (@emillon)
- some progress at JS regarding dune losing parallelism, and caching of directory targets. these will be upstreamed soon