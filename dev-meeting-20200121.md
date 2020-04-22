## Polling mode

Andrey is continuing his work on making the polling mode be fast by
taking advantage of the memoization system. He is currently looking at
places that are using ad-hoc caches and trying to make them use the
main memoization system. This needs to be done as these ad-hoc caches
won't play nicely with the main memoization system once we start
keeping results between runs. Indeed, these ad-hoc caches don't track
the dependencies of computation as the main memoization system does.

## Mdx

Nathan is still working on adding a mdx extension to dune. Currently
mdx can generate Dune rules that user can include in their dune
files. However, the setup is a bit tedious to work with. This work
will in particular benefit Real World OCaml, which is a heavy user of
mdx and could also help streamline mdx. Among other things, mdx allows
to have verified top-level sessions in .md or documentation comments
in .mli files. We expect that at least the latter will be useful for
pretty-much all OCaml projects.

One thing Nathan is currently looking at is clearly delimiting the
scope of what we want to support. Indeed mdx supports quite a lot of
features and supporting everything would be too complicated and not
worth it.

## More strict checking of extensions versioning

Right now, dune doesn't check that the version of extensions (as in
\`(using menhir x.y)\`) is supported by the version of dune specified
in \`(lang dune x.y)\`. As a result, some errors are only detected
much later on during the testing done by the opam team.

Ulysse is looking at changing that. The goal is for Dune to know the
relation between the version of extensions and the version of Dune, so
that such errors are detected at development time. Such errors are
fixed by raising the version of the dune language.

Eventually we expect that extensions might live outside of the Dune
repository. However, because we currently don't have a good story for
extending Dune from the outside, they live inside the Dune repo and so
they are tied to a particular version of Dune.

## Coq support

Coq support is going well. Emilio is interested in two topics:

- support for rules with targets in different directories
- support for extensions living outside of the Dune repository

For the former, there is no theoritical blocker. We talked about it in
the past and we have a few ideas how we could allow it without
penalising performances. For instance, we could allow a rule to
generate targets only in the current directory or in
sub-directories. This way, to know what might produce a file, we only
need to look at the current directory and its ancestors, rather than
potentially all directories in the workspace. That would be a good
project for the retreat.

Regarding extensions, this topic has been coming up for a while. We
plan to talk more about it during the retreat. Emilio mentions that he
has two ways of building Coq projects with Dune:

- one where the system generates plain `dune` files and `dune` itself
  doesn't need to know about Coq
- one where the Coq support is builtin `dune` and has access to the
  internal and unstable APIs of Dune.

We said that it would be interesting to compare the performances of
the two approaches.

## Terminal support

Jeremie would like to teach Dune how to control the terminal, so that
we can create full-terminal UIs. For the watch mode or for a more
fancy command line construction interface for instance. The goal is to
provide something relatively simple that could then be extended. This
would likely be an aspect of Dune that would be easy and fun to
contribute too. Andrey mentions that other modern build systems can do
this and in particular mentions an interesting feature of bazel where
it shows the jobs being executed. It is then very easy to catch
commands that takes too long to execute because they stay for a while
on the screen. That seems like a nice feature to implement in dune as
well.

As Etienne points out, it would also be nice to have a RPC so that
such interfaces can be written outside of Dune.

Arseniy also mentions that it might be complicated to support
everything that is supported with the simpler current interface for
the watch mode, so we shouldn't get rid of it.
