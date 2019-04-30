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

Conclusion: let's ask the community what versions of OCaml they care
about before doing anything. With a bit of luck, we don't need to care
about 4.02 anymore.

## Cram testing

We have a small cram testing framework in dune that is really useful
to write integration tests. There is also the
[craml](https://github.com/ocaml/dune.wiki.git) project which might be
a more polished version.

Since this kind of testing would be useful in many projects, it would
be nice to formalize them a bit more and make them easy to use in dune
projects.

We talked about having a light plugin system in Dune which would cover
this. Though having something specific to cram tests might make sense
anyway and would take less time to integrate.

Conclusion: not discussed

## Testing in the CI

what is the recommended way to run tests in the CI?

Related to #2082.

Conclusion: not sure how much it's worth doing. They are many
different ways to define tests and enforcing that all tests are
attached to a package might be a lot of churn. We need more feedback
and numbers to justify being strict about attachement of tests to
packages.

## Library variants

There is a problem at the moment with library variants: the choice of
implementation depends on what's installed on the system, which is not
great. We are going to limit the feature to the strict minimum,
i.e. one will only be able to declare variants for virtual libraries
in the same project. We will then wait for usage feedback.

## Opam generation

The feature is reaching completion. Before releasing, we want to make
sure that dune can generate the whole opam file. The last fields dune
can't generate at the moment are `build` and `depopts`. We are going
to add support for specifying these in the `dune-project` file.

## Coq

Work is continuing to make Dune support Coq. The design is still being
worked on.

## Distributing dune

We'd like to start distributing the dune binary via more distributions
platforms such a homebrew. This would be only the dune binary, not
accompanying libraries such as `dune.configurator`.
