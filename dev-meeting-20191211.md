## Work on performances

Work is being carried out at Jane Street to make the polling mode of
Dune faster. A large part of JS internal mono-repository can now be
built with Dune and this is what JS devs are using to work on
performances.

One interesting detail is that spacetime was successfully used to
identify bottlenecks in Dune, which were then easily fixed using the
memoisation system.

## Faster digestion of data structures

We do a lot of marshal+digest in dune to compute the md5 of complex
data structures. Several devs have noticed that this is slow and
putting pressure on the GC.

We discussed how this could be improved. First, we could replace md5
by something else. Nowadays there are better and faster digests. Now
that we are allowing ourselves to use C in Dune, this shouldn't be
difficult.

Second, we could switch to a streaming interface to compute the
digest, so that we don't have to construct an intermediate marshaled
representation. Using `ppx_hash` + `[@@deriving_inline]` seems to be
the easiest and fastest way to reach that goal and agreed that we
should give it a try.

@aalekseyev also mentioned that we could play with the C API of
`Marshal` and `Digest` to get a streaming behaviour with the current
method, though the `ppx_hash` way seems more future proof.

## Conflicting library versions and big workspaces

Especially in the duniverse context, one might want to have tools such
as `ocamlformat` be part of the workspace. However, this create one
issue: ocamlformat uses `Base` and the things being developed in the
workspace might use `Base` as well, but at a different version.

We could extend Dune scoping so that this is allowed. Given that the
two versions of `Base` will never end up being linked in the same
executable, that should work just fine.

## Misc

The last topic led to a discussion about `duniverse` and `dune`. We
wonder whether we could have a world where `duniverse` could prepare a
workspace that would be immediately consumable by a plain `dune`
binary, and without physically vendoring all the depencencies in the
repository. For instance, by only storing pointers in the workspace
that `dune` would resovle to some central cache automatically managed
and filled on demand. It's easy to construct an "expanded" view of the
world in Dune and we already do it for symlinks for instance.

@rgrinberg pointed out that one advantage of having `duniverse` become
the main using facing tool is that it allows to pin a particular
version of `dune`. This is sometimes needed because projects depend on
a particular behavior of one version of Dune.
