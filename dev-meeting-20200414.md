## merlin/dune interaction

Ulysse is working a new way for Dune and merlin to interact. Instead
of Dune generating `.merlin` files in the source tree, merlin will
know call a command `dune ocaml-merlin` that will start a RPC via
stdin/stdout. Merlin will ask Dune for the merlin configuration on a
file-by-file basis.

This will have two advantages over the current setup:

- we will no longer generate `.merlin` files in the source tree, which
  is something annoys users
- we will be able to provide more fine-grained per-file configurations to merlin

As soon as this is supported, we will stop generating `.merlin` files
in the source tree.

On the dune's implementation side, we will continue to dump some
merlin related configuration on disk during normal builds, and `dune
ocaml-merlin` will simply read this information. Eventually, we might
migrate to a more straightforward design where `dune ocaml-merlin`
communicates with a dune in watch mode to obtain this information.

## Link time code/gen

Currently, one can link against `dune-build-info` to capture various
information such as version information from the VCS. This works well
and doesn't break incremental builds.

However, many big users already have their own scripts to capture
various custom things at link time and `dune-build-info` is not enough
for them.  We would like to provide some customisation to enable users
to use a mechanism similar to `dune-build-info` but with their own
scripts.

Ulysse will be working on this feature.

## enabled\_if in install stanza

We currently support a `enabled_if` field in various stanzas to enable
them conditionally. Ulysse will add support for this stanza in
`install` stanzas.

Eventually, we should really have a top-level general `if`
stanza. Andrey mentioned that it would be nice to have a clear design
document describing where we are going with the Dune language. Jeremie
agrees, and this will especially be important we will let users extend
the Dune language. For now, we should continue extending the language
when necessary, but making sure we don't extend it in ways that would
be difficult to formalise. So for instance, adding user-defined
variables is currently a no-no.

## Dune cache support in Jenga

Andrey is working on making Jenga support the Dune shared cache
format. In the process, he encountered some missing features, such as
the ability to store command outputs. Indeed, Jenga allows to cache
the output of a command but Dune doesn't. For instance, in Jenga we
cache the output of `ocamldep` while in Dune we dump it to a file via
a classic rule.

Andrey would also like to dissociate the versioning of the metadata
and the file store, which seems reasonable. To be continued.

## Directory targets

Andrey reminds us that he wrote a RFC for adding support for directory
targets in Dune. Everyone is invited to have a look and comment on the
[RFC][rfc]

And in general, everyone is invited to read RFCs, give their honest
opinion and ask for precisions.

## Foreign executables

Nicolás asked people's opinion on a `foreign_executables` stanza, to
build an executable from C or another foreign language.

In principle, that seems fine, however there is likely to be a lot of
complexity around this. All compilers work differently, take different
options and this is not knowledge we necessariliy want to add in Dune
at the moment. For now, we decided to do nothing in this direction.

## C-sharp

Nicolás also asked about supporting the `C#` language in
`foreign_archives`. Everyone agrees, this seems to fit well in the
design as long as compiling `C#` is similar enough to C, which Nicolás
confirms.

## Pipe action

Nathan is working on adding support for a `pipe` action in Dune. This
will be implemented with a temporary file rather than a real pipe,
however it will still be convenient for users.

[rfc]: https://github.com/ocaml/dune/issues/3316
