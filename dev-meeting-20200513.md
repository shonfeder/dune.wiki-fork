Present at the meeting:

- Andrey Mokhov (@snowleopard)
- Anil Madhavapeddy (@avsm)
- Emilio Jesús Gallego Arias (@ejgallego)
- François Bobot (@bobot)
- Jérémie Dimino (@jeremiedimino)
- Rudi Grinberg (@rgrinberg)

### Management of repositories under ocaml-dune

In the past we have had a few issues related to `dune-configurator`:
- old versions of `dune-configurator` don't work with recent versions of Dune
- new versions of `dune-configurator` don't work with older versions of OCaml

Which makes it impossible for a project to:
- use `dune-configurator`
- keep compatibility with older versions of OCaml
- use recent dune features

This is unfortunate because this scenario is not that rare. Indeed, a
lot of project want to support a wide range of OCaml versions in order
to maximise the reah of their software. They also want to use recent
dune features because we keep adding useful features. And if they need
to query some C related settings, `dune-configurator` comes in very
handy.

The reason old versions of `dune-configurato` don't work with recent
versions of Dune is because there is a bit of communication at runtime
between Dune and OCaml and this communication is not versioned.

Making `dune-configurator` work with older versions of OCaml is
difficult because it depends on the Dune private libraries and we'd
like the freedom to rely on recent OCaml features in the Dune code
base. This is usually fine given that we only provide a binary and the
OCaml version used to build it is just an implementation detail,
however this is not fine for libraries.

At a high-level, the relation ship between `dune` and
`dune-configurator` is not well defined and versioned. Fundamentally,
the only thing Configurator needs from Dune is the configuration of
the current build context. If we limit this relationship to just that,
it becomes easy to version it and add more flexibility between the
versions of Dune and Configurator.

To enforce this, we decided to treat Configurator as an external
project and extract into a separate repository, forcing us to make the
relationship between the two project more explicit and controllable.

Dropping the dependency between Configurator and the private libraries
of Dune caused us to duplicate some code from Stdune into
Configurator, and we discussed what we could do to avoid this
duplication.

The obvious idea would be to extract Stdune into its own repository
and package. What is more, some other projects such as
[OCaml-LSP](https://github.com/ocaml/ocaml-lsp) are already vendoring
Stdune and sharing it more officially could be nice for such
projects. However, the API of Stdune is way too big for us to commit
to its stability, which is obviously a problem. We don't want to end
up in a situation where making a new release of `dune-configurator`
splits the universe because the API of Stdune broke.

Jérémie proposed to avoid such problems by putting the major version
of the library in the package name.  i.e. we would have `stdune1`,
`stdune2`, and so on. This way, we could make new major release of
`stdune` as often as we would like without breaking the world.

Anil mentions that this is not going to be great for opam. Emilio
replies that this is what Linux distributions are doing. After some
more braistorming and various other solutions proposed there is no
consensus. It is not clear whether some of the solution proposed will
solve the original problem, and also what will be the damage in terms
of workflow.

For now, we decided to write up a bit more about the various solutions
so that we can reason about them.

### cmt files problem

There are various issues related to the way cmt files are produced:

- https://github.com/ocaml/dune/issues/3182
- https://github.com/ocaml/dune/issues/3467

Currently, cmt files are produced when comping the code in
bytecode. This has a few consequences:

- for an executable or library that has `(modes native)`, `dune build
  @all` will not build the cmt files :(
- for a public executable or lilbrary that has `(modes native)`, `dune
  build @all` will build the bytecode versions just for the cmt files

We could produce the cmt files using dedicated build rules that only
produce the cmt files by using `-stop-after typing`, however this
might make builds slower.

We decided to change our build rules so that when bytecode compilation
is not requested via the `(modes ...)` field, the cmt files are
produced by native compilation indeed. This should fix both problems.

Rudi also noted that it would be nice if we could restart compilation
from cmt files, which would both solve this problem in a nice way and
avoid the problem where we get warnings twice; once from `ocamlc` and
once from `ocamlopt`.

Jérémie pointed out that eventually cmt files won't be necessary for
Merlin anymore. Indeed Thomas Refis and Leo White have a plan to make
cmi files sufficient for Merlin. When this happens, we could
reconsider building and installing cmt files given that they are the
biggest kind of artifacts. Anil points out that this is not that
simple because odig needs them for odoc.

### François sites and relocation PR

François talked about his PR for sites and relocation:

https://github.com/ocaml/dune/pull/3104

This PR implements a feature needed by real world programs so this
fits well into Dune.  However, it is quite big and there are a lot of
design questions.  To proceed, we will need to have a few people look
at the high level design and play with the feature.  Once this review
staged has finished and we have reached a consensus, we can proceed
with the technical code review. Emilio says that he is willing to try
out the feature and Jérémie suggests that David and Nicolás might be
good persons to ask as well.

Because this feature is touching various part of Dune and adding some
code for better interop with findlib, Jérémie asked François if he
could send a small summary of the technical changes to the code base,
in order to asses the change from a maintenance/sustainability point
of view.

### OCaml and Rust: Cargo and Dune Integration

A few users are interested in using Rust code in their OCaml
projects. Rust code builds with Cargo, which is the equivalent of
opam+dune in the Rust world. There is a [thread on discuss](https://discuss.ocaml.org/t/cargo-opam-packaging-of-a-rust-ocaml-project/5743/)
about this, and an [example project on github](https://github.com/zshipko/ocaml-rust-starter).

Anil mentions the idea of treating the Rust build in a similar fashion
to the [foreign build sandboxing method](https://dune.readthedocs.io/en/stable/foreign-code.html#foreign-build-sandboxing).

Jérémie asked what the story will be when the user is in the editor
and working on both the Rust and OCaml code. It feels like that there
should be a bit more cooperation between Cargo and Dune in order for
things to go smootlhy. Anil that he knows someone who should start
working on this question soon.
