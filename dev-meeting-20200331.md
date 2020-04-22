## Issue management

We discussed the fact that sometimes, we reach an agreement on what to
do for an issue but then nothing happens. This is not the end of the
world, but just to make things clearer for everybody we proposed the
following workflow change: when we have reached an agreement we will
add the tag "accepted" to the feature and also add a comment with a
link pointing to a description of what the "accepted" tag means.

The accepted tag will mean: the feature request is accepted. If anyone
wants to make it happen, please raise your hand and we will happily
give you some pointers. Otherwise, maybe someone from the dune team
will pick up the issue at some point.

We will then review the list of issues with the "accepted" tag once a
year, and for each issue decide whether to close it or leave it in
this state. If we leave it in this state, we'll leave a message on the
issue to indicate that it has been reviewed and kept in this state.

## Chdir

Rudi has been working on a feature to inject stanzas into a
sub-directory from a dune file. Jeremie initially suggested to call
"chdir" to match the "chdir" action because he had in mind a
formalisation of variable elimation in dune files where "chdir" was a
general modifier.

However, while discussing we realised that this causes a discrepency
between the way `%{env:xxx}` variable are scoped inside `chdir`
actions and `chdir` stanzas. In the first case they are lexically
scoped while in the second case they are not. For this reason, we
decided to use a different name for the stanza.

## Language specific qualified/unqualified statuses

Right now, the qualified/unqualified choice is for all languages and
this is a problem. This problem was revealed for Coq, since Coq uses
the qualified semantic and OCaml only support the unqualified one. And
even if OCaml supported both, we might want different settings for Coq
and OCaml when extracting OCaml code from Coq.

In the end, we agreed that for most language the choice is made once
and for all by the language itself. It is only for OCaml where the
user can make this choice. So in the end we decided to de-couple both
settings. `include_subdirs` will now have two state: `true` and
`false`, and in the `true` case we can additionally specify whether
OCaml sources are qualified or not. For instance:

    (include\_subdirs true (ocaml unqualified))

We should add the new syntax and deprecated the old one. In version 3
of the language, we'll forbid the old syntax.

## Directory targets

The previous point led to a discussion about directory targets. We
confirmed that currently they are not officially supported but
discussed a few ideas for how they could be supported. Andrey
mentioned that Microsoft have discussed this problem and consider it
solved. There solution is well documented, so we should look at what
they are doing. Once we have a clear idea of what directory targets
mean, we will formalise this in Dune and add proper support.

Right now, even though directory targets are not officially supported
and formalised, you can hack together rules that do produce
directories. This however doesn't work when using the shared
cache. Quentin proposed a simple solution to make them work with the
shared cache, by simply not caching rules that produce directories.

## Distributed cache

Quentin added support for storing in each cache entry the commit id at
which the artifacts where produced. He will then look at a way of
indexing those so that we can go from a commit id to a set of
artifacts.

This could be useful to say: I checked out this revision, prefetch all
artifacts that were built at this revision.

## ocamlc -i

Nathan is trying to make it easier for user to extract the inferred
mli out of a ml file in a dune project. This is a long standing
request.

He is currently working on a third-party tool that invokes dune,
figure out the location of the right cmi file and extract the inferred
mli from the cmi file. The it is done is pretty hacky, with the tool
parsing the verbose output of dune to try and figure out the location
of the cmi file.

David mentions that if it was a sub-command of Dune, it would be more
discoverable to users. Jeremie agrees but is a bothered by the fact
that this command is very OCaml specific, while the rest of the Dune
CLI is pretty generic. In the end, we concluded that it would be nice
to have an "ocaml" command to regroup various OCaml specific commands,
such as `dune ocaml print-intf` or `dune ocaml top`.

Once Cmdliner supports sub-commands, we can do that and bring this
feature in Dune.

## Better linting pipeline

Nathan is currently working on formalising the linting pipeline. Right
now, it is possible to use several linters such as:

- cinaps
- mdx
- deriving\_inline
- ocamlformat

Right now what happens is that each linter independently checks the
input and produces a correction. This results in a weird workflow
where several linters might produce incompatible corrections.

To make things better, we plan to instead order the various linter
into a straight pipeline, where each linter consumes the output of the
previous one. And only at the end, after applying ocamlformat as the
last linter we compare the correction to the original source file.

## j and pipe

The previous discussion led to discussing support for Dune setting up
pipes.  In the past, we decided against adding a `(pipe a b)` action
in Dune because it is not clear whether `(pipe (run ..) (run ...))`
should count as 1 job or 2 jobs. Arseniy mentions that a job never
really meant anything and we could decide to count this as 1 job. In
the end, we kept the status quo on this.
