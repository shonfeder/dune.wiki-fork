Present: @emillon @maiste @nojb @moyodiallo @ManasJayanth @Leonidas-from-XIV @gridbugs

## Work presentation tour

- @maiste: working on the developer preview distribution.
- @gridbugs: gets to merge the `toolchains-minimal` branch.
- @emillon: moving dune to use 5.1 (and later 5.2).
- @nojb: supervised an intern working on a better granularity of the module system in _Dune_.
- @ManasJayanth: here to look at what can be done on Dune. Working on _Ezy_.
- @moyodiallo: working on having _OCamlformat_ available when using _toolchains_.

## @gridbugs - Toolchains

- It is using the already existing `Config` mechanism to enable _toolchains_.
- A patch can be made for the developer preview to enable it by default.
- It installs the compiler based on what is in the lock file.
- It works like a regular package but with a different prefix location.
- It uses a mechanism to build the compiler in a temporary directory but makes it believe it is built in a specific repository. In case of interruption, it's considered as not installed.
- It will be easier when the relocatable compiler is available.
- _Ezy_ solves the problem by considering the compiler as a particular package.
- _Dune_ also has location issues with `Cmt` files as they are compressed. Thus, the location is not available.

## @emillon - Upgrade OCaml version

- Needs to be done, as the more we wait, the more time it will take.
- Some test dependencies prevented the upgrading and needed to be fixed first.
- Upgrading to 5.1 and then upgrading to 5.2 later:
- _OCaml_ 5.2 makes _Dune_ emits some alerts and fail. The semantics of alerts is different. Maybe, it is a regression in _OCaml_.
- The alert happens in `stdune` when the code runs outside the module.

## @gridbugs - Lock file discussion

- Context:
    - _Dune_ creates a lock directory with lock files for package management.
    - It provides information about how to build packages and some metadata.
    - The lock files are system-dependent.
- Should the lock file be checked in using a version control system (as you would do with _NPM_)? It does not guarantee the build reproduction on other systems and platforms.
- A workaround is to specify the lock file for each platform in the `dune-workspace`
- How does _Rust_ solve the platform-specific problem:
    - The platform and system are discovered at build time and the code is built depending on the code-specific parts (preprocessing).
    - An issue with _OCaml_, as the compilers have some dependencies platform-related.
    - Maybe, _Dune_ package management lock files doesn't need that much flexibility.
- The DSL for packages allows them to have conditional variables. Extracting the conditions from _Opam_ has not been retained as a solution: _Opam_ variables are evaluated early, and it can lead the lock files to have different dependencies on different systems.
- Difficulty with lock file per platform:
    - It would mean many locked directories.
    - The default behavior is not to do this. People are not expecting it to be platform-specific.
    - It needs to compute all the solutions, so it would be slow (and not implemented in the solver actually).
    - Multiples versions of the packages in the same environment, but it's not supported.
    - The lock files are designed to not interact with _Opam_ (get the sources).
    - However, some packages are still platform-specific (e.g. _Eio_ backend).
- What are the implications of using a lock file instead of a lock directory?
- _Opam_ `extra-files` creates some problems.
- However, _Opam_ is moving away from `extra-files` to `extra-sources`. As an example, Opam monorepo does not use extra files
- It is more complicated to review the lock file.
- However, an advantage is to discourage people from updating their lock file manually.
- Less conflict, even if, in case of conflict, people could regenerate the lock file. This is what happens in other languages / build systems.
- Asking developers if they would be interested in reviewing changes in the lock file. It can give an idea about the lock file usage.
- _Dune_ can provide a first implementation and the lock file semantic can change as it would be an unstable feature.
- Plans:
	- Change the name to a hidden directory
	- Documentation would specify that it is platform-specific and that there is a way to fix the problem (using the `dune-workspace`)
	- Some tests can be run by building a simple toy project and porting it to multiple platforms to see if it works.

- References for other build systems:
	- https://github.com/grain-lang/grain
    - https://github.com/esy/esy/pull/1349