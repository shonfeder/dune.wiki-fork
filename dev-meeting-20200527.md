Present at the meeting:

- Arseniy Alekseyev (@aalekseyev)
- François Bobot (@bobot)
- Jérémie Dimino (@jeremiedimino)
- Emilio Jesús Gallego Arias (@ejgallego)
- Andrey Mokhov (@snowleopard)
- Nicolás Ojeda Bär (@nojb)

### Protocol for communication with the shared build cache:

Andrey mentions that Jenga has seen some useful updates on its caching
protocol; it would make sense propagating some of the improvements to
Dune.

### Sites PR (https://github.com/ocaml/dune/pull/3104)

Last week David, François, and Emilio met to discuss the Sites PR. The
PR is still in draft state but a revision should come in June and
hopefully become a merge candidate.

Emilio noted that the PR seems to be providing a DB that could be
useful for the Coq subsystem, dropping then its custom `Coq_db.t`
placed in the super context. This won't happen immediately, but after
the merge. Likely some extensions would be needed as to allow to
attach metadata to particular sites, but the DB could be a good
opportunity to unify resolution rules [installed libs, workspace
public libs, private, per scope libs].

Another question regarding the PR was whether it could be useful for
Lexifi / Windows. As of today it seems the PR is too Unix-specific to
be of immediate use, but Nicolás shall look again into the pull
request to respond the question better. Jeremy mentions that the fact
that the functionality is, at first, Unix-only may not be a merge
blocker.

In particular, Unix conventions are not always adequate on Win; which
it usually much more lax about where stuff goes. It is noted that the
current plugin system [using ocamlfind] does work OK, so at least the
replacement provided here could be made to work too.

Frama-C will be a key user of the sites PR, for Windows the plan is
always use relocation, where the sites are looked up relatively to where
the executable is.

It is noted that the sites PR doesn't use findlib anymore to locate
plugins, however it does piggyback on the META file format as to
introduce less friction / configuration formats saturation.

The META files are placed not in the global namespace.

One key point to move away from findlib is that it loads transitive
dependencies, but some use cases don't want that. Some other little
things are also simpler with a fresh implementation.

In the "sites" model, `OCAMLPATH` is used to put the relative
directories in scope; this is done by the sites code before calling
Dynlink.

One non-optimal thing is that the regular `META` parser does depend on
`stdune` which is a bit heavy; the current implementation uses a
functor to reduce the size, but it may need further tweaking. Another
choice is to make the `META` parser standalone, but this is a bit of
effort.

### Build profiles and modes

We discussed a bit the use cases for profiles and their interaction
with modes [`native` / `byte`]

For example in Coq, developers often want several modes with different
proof checking behavior; sometimes skipping proofs, or generation of
OCaml code for the evaluator.

Coq has an "OCaml evaluator" where Coq files are compiled to `cmxs`
plugins and can run programs much faster. This is heavy, so it is only
enabled by user request using the `(modes ...)` field of `(coq.theory
...)` When in `dev` profile, users want to skip this step [and only
generate/install these files on a release profile]

This implies that the `dev` profile would modify what Coq modes are
available. Once you have composition, reasoning about all cases may
become tricky.

In general, it seems that profiles should fit well several languages,
thus, `dev` and `release` are common to Dune, and each backend
implements them. Backends could also understand extra profiles, but in
general Dune should provide a good set of built-in profiles.

The Coq backend will try to experiment on this area, but it'd be great
for other devs to keep an eye to see if Coq is making bad use of
profiles or modes.

It is observed that mode is indeed something more specific to install,
vs profiles which are set of settings.

Some discussion about having a profile to only build native targets on
`@all` happened, to be continued; for now the workaround is to
explicitly list deps.

### Users of Coq backend

We wondered if Dune is being used by Coq users, the answer seems
affirmative; despite there has not been an official call for testers
yet, some features such as extraction or composition have attracted
some advanced users; Dune is also very convenient for the OCaml part
and OPAM support, which is the common package distribution method in
Coq.

Some users are Clément-Pit Claudel @ MIT, Karl Palmskog, Thomas Letan;
and of course the Coq devs themselves. coq-community will also try to
migrate a good chunk of packages to dune + dune-release once we
announce the call for beta testing.
