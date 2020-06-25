## Present at the Meeting:

- Arseniy Alekseyev (@aalekseyev)
- Jérémie Dimino (@jeremiedimino)
- Emilio Jesús Gallego Arias (@ejgallego)
- Andrey Mokhov (@snowleopard)

## Vendored libraries in developer build

Jeremy proposes to change how vendored libraries are handled on
developer build, example: https://github.com/ocaml/dune/pull/3575

The main idea is that in bootstrap / release, the vendored copies will
be used, but for development, dune will depend on the installed
versions of such libs.

This should solve some annoying clashes of system vs vendored libs,
for example in tests.

It should avoid some renaming, which is also annoying, and quite a bit
of work to do when adding new deps.

The main dev-visible changes are:

  - a complete install of dependencies is now needed for development,
    vs the current setup which only requires some deps
  - no more modifications to vendor allowed, patches should go upstream

## Jenga / Dune convergence

Recent work in Jenga has highlighted some interesting improvement
which could benefit Dune.

## Dropping support for 4.07

4.10 seems ready as dev platform, thus following the current compat
policy support for 4.07 should be dropped.

`configurator` setup has been improved, this should mean people on
older OCaml + `ocaml-secondary-compiler` are fine.

Move to 4.08 is interesting as the custom `future_syntax` preprocessor
goes away. This will enable some nice use cases such as instrumenting
dune itself with `bisect_ppx`.

## New build system API: collecting thoughts, ideas and wishes

https://github.com/ocaml/dune/issues/3571

Quite open at this point; the goal is collect ideas and brainstorm a bit.

For example "writing a rule is annoying because of this problem" kind of feedback.

## Coq data

Emilio did collect a list of people using Dune for Coq, it will eventually appear at https://github.com/ejgallego/coq-plugin-template .

Lots of discussion among Coq devs about how to better support "Coq's native compilation" still didn't converge, but progress has been made.