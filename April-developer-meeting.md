Proposed discussion topics for this upcoming meeting.

## Extending future_syntax

It is quite annoying to not be able to use new OCaml features in dune
or other platform tools. Indeed, very often these tools need to stay
compatible with older compiler. The `future_syntax` feature comes in
handy.

@diml proposes that we extend this feature to cover more language
features, such as inline records. This would be especially relevant
for the work being done in ppxlib, as using inline records in the
representation of the stable AST would be very relevant.

If we want to support such features, we can no longer rely on the same
mechanism we used for `let+`. Instead, we need to have our own OCaml
parser in Dune. As usual, with recent versions of OCaml
`future_syntax` will do nothing, and with older ones, it will be a
preprocessor that uses our own parser. This parser will basically use
the lexical conventions and grammar of the last version of OCaml, but
will produce an AST that is accepted by the version of OCaml in use.

This will add a bit more code to dune, though the value seems worth
it.

## stdune-v1

In other platform tools and libraries, we have similar needs to what
we have in dune. In particular, we'd like to be compatible with
multiple versions of OCaml. This is one thing `stdune` is good at: it
provides a set of functionalities that work across a wide range of
OCaml versions.

I'd like that we share this work so that we don't have to do exactly
the same thing in every platform project. At the same time, we don't
want to over-commit on these API. So I suggest that we do the
following: we add a library `Stdune_v1` in the dune repository,
released as a separate package `stdune-v1` that will be an thin stable
layer on top of `stdune`. We will expose only the functionalities that
we are completely happy with. At least initially, we will not expose
modules such as `Path`. To make it clear what `stdune-v1` provides,
the mli should be fully expansed and not rely on inclusion of upstream
APIs.

This is a bit more work, but on the other hand, when users want a bit
more functionalities, such as `Path` for instance, we can ask that
they do some refactoring work on it so that it reaches a state that we
are happy to share. Such work could benefit the dune code base.
