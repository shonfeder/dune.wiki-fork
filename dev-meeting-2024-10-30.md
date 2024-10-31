# Agenda

- Release 3.16.1
- Is there a way to define a true alias without having to insert it in a rule?

## Release 3.1.6.1 (@maiste)

We are releasing this patch version of dune `3.16` to incorporate a small change necessary for the `beta` version of `ocaml.5.3.0`. This is in a good way, and most of it went smooth. We need to discuss the `3.17` release because it will take more time to be release as there will be some regressions.

## Alias in dune (@maiste)

- Having a generalised method is not needed because in terms of `dune`, an `alias` is a reference inside a directory.
- For `@pkg-deps`, we want the alias to be either at toplevel (workspace) or attached to a directory (here `dune.lock`).
  - We want it to use the targets as a dependency, not the source as it is the case today.
  - We also need to take care about the extra sources from opam as dependencies.
  - Building the archive means downloading it and then build it.
- When attaching alias, it has to be done on the source tree, otherwise dune won't be able to find it.
- Cookies for dune are more or less a binary version of the `.install` file.
- We can change the name to `pkg-install` instead of `pkg-deps`. Would be better as it would be more related to the `@install` alias.

## Disjunction (@Leonidas-from-XIV)

When there is a disjunction in opam file (using `or`), in the current situation, we need to choose one package. A solution would be to warn the user that we can select two options and request that the user chooses by explicitly adding the dependency.

## "Unvendor" the 0install solver (@ElectreAAS)

To give us more control over the solver and the errors printed, we have decided to extract it and have our "own" version of the 0install solver.

