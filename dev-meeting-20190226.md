## Dune support for Coq

@ejgallego is working on adding support for building Coq projects with
Dune. This requires some modifications to Coq itself but it is
progressing.


## Dune plugins

[https://github.com/ocaml/dune/issues/1855][1]

Dune developers are generally happy with the proposal. @avsm mentions
that plugins have been a failure in every OCaml tool and dune might
know the same fate if we introduce plugins. On the other hand, there
are enough compelling use case to justify providing some form of
extensibility in dune. Using promotion to commit the output of plugins
would leverage the issues, but we are not sure how bad it would impact
their usability if we madee this the default, so we'll need to
experiment.

We agree to go ahead with plugins, but make them an experimental
feature at first.

@rgrinberg and @avsm mentions that there are a few additional features
that are needed to make plugins cover the use cases we have in mind:

- the ability to specify stanzas for other directories, in particular
  generated ones. This is useful for ctypes
- some way to have automatic stanzas based on the contents of the
  directory. For instance a way to say that every directory with a
  `run.t` file automatically gets a `(dune_test)` stanza, where
  `dune_test` would be defined by a plugin

## Mdx support

We would like to provide better support for mdx in dune. Because mdx
files are often part of a library and share its build configuration,
i.e. in terms of dependencies and preprocessors, it seems natural to
add a field to the `library` stanza to allow specifying a list of .md
files that should be updated by `runtest`.

Sometimes these should also be part of the generated HTML
documentation. We will need to figure out a way to specify this.

@avsm mentions that there is a similar question with odoc and code
snippets in odoc comments. However, upstream is not decided on what to
do so there is not much we can do on the dune side for now.

## Configuration variables

We have been discussin for a while the possibility of having
configuration variables in dune. Mirage is likely to be the biggest
consumer of this feature so configuration variables are currently
being experimented with in the context of mirage.

## Warning 3 being a fatal warning in dev mode

The general consensus is that the status quo is acceptable, though it
does feel like it would be nice to have a mode in between release and
dev where we don't change the settings but still get the damn
executable even if the compiler has remarks about our code. Some kind
of `dev-loose` mode for instance. Better editor support would also
help in order to quickly switch between modes.

[1]: https://github.com/ocaml/dune/issues/1855
