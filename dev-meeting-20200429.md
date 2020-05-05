This document is still a work in progress.

Present at the meeting:

- Andrey Mokhov (@snowleopard)
- Arseniy Alekseyev (@aalekseyev)
- Emilio Jesús Gallego Arias (@ejgallego)
- Jérémie Dimino (@jeremiedimino)
- Nicolás Ojeda Bär (@nojb)
- Rudi Grinberg (@rgrinberg)
- Ulysse Gérard (@voodoos)

## merlin/dune interaction

[Previous notes](dev-meeting-20200414#merlindune-interaction)

Ulysse has a working prototype that is new under review.

## Link time code/gen

[Previous notes](dev-meeting-20200414#link-time-codegen)

Ulysse made progress on this feature. While discussing how to make
this play well with incremental builds, we found a simpler design that
would reuse the existing `dune-build-info` library and its machinery.

The idea is that a user would attach to a library an action to produce
arbitrary data at link time, and the programmer could recover it via a
new function in `Dune_build_info`:

```ocaml
(** Return the data produced at link time for library [library_name].
    Return [None] if the executable is called from [_build] where the
    link time code gen data have not been filled in order to not penalise
    incremental compilation *)
val get_user_data : library_name:string -> string option
```

It would then be up to users to encode and decode the information they
want to capture at link time.

We plan to go for this design.

## Dune cache support in Jenga

[Previous notes](dev-meeting-20200414#dune-cache-support-in-jenga)

Andrey made the change to dissociate the versionning of the metadata
and the file store. While working on the shared cache, he also
simplified a few things, such as:

- remove extra paths in file names from the protocol and metadata
  files that could easily be recovered from other fields
- remove the colision handling mechanism which feels complicated and
  non-necessary (Jérémie who proposed it should know its stats better)

Andrey also added support for this new cache format in Jenga and will
move forward with switching the format used by Jenga by default, which
is a step towards using the dune distributed shared cache in Jenga.

## Language evolution

Everyone agrees the dune language is evolving in ad-hoc ways without a
clear direction, and some parts are kind of out of control. This is
unlikely to change in the short/medium term, but we should think about
giving a clear semantic and direction to the language in the long
term. This might require deeply changing some aspects of the language.

The JSON topic was also brought up. JSON as become a standard and
using JSON would mean that we would benefit from existing tooling
built around it.  Jérémie is planning to generalise the Dune's parsing
engine, which should give us a JSON syntax for free. However, the
syntax of the action DSL will be pretty ugly. Variables might also be
a bit messy.

To be continued.

## Dune fmt

We agreed that the pretty-printing of dune files should be versionned
just like many other aspects of Dune. This mean that if one day we
change the formatting of dune files, we will need to keep support for
the old versionning. But given that the syntax of dune files is
relatively simple, this should be manageable.

A consequence of this is that the way dune files are formatter is a
pure function of the dune language version. As a result, it doesn't
matter whether we call the `dune` binary in the `PATH`, the current
`dune` binary or even if we do the formatting in-process given that
they will all behave the same.

## Generalisation of bisect support

Dune is gaining builtin support for `bisect_ppx`, a code coverage tool
implemented via a ppx rewriter. Nicolás mentions that it would be nice
if such support could be generalised to all ppx instrumentation tools.

Everyone agrees.

To do that, the first step is to look at how all these tools work and
find what is the underlying general mode of operation. Jérémie asks
Nicolás if he could look into it given his interest for `landmarks`.
Nicolás makes no promise, but said he will try to look into it.

## Fiberisation

Rudi is still working on spreading the Fiber through the Dune code
base, which is a necessary step for future exciting new features.

## Coq

Emilio tells us that a lot of Coq projects have been ported to Dune
and that they will soon issue a call for Beta testers.

The Coq team is also interested in bringing incrementally right into
the core of Coq, possibly reusing some of the technologies developed
in Dune. Apparently, the Isabelle proof assisitant does something like
this, and a few of us also heard that the Rust compiler is doing
something similar as well.

## intra-process timings reporting via catapult

Dune has a `--trace-file` option to report various measurements via
the catapult format. Recently, something similar was added to the
compiler itself.

Emilio mentions that it would be nice if `dune` could collect and such
intra-process reports. To be continued
[here](https://github.com/ocaml/dune/issues/3449).

Jérémie also mentions that inside Jane Street there is a system called
"Telemetry" to send various data from a process to a server that
collects them for later analysis. The system has very low overhead and
for instances sends the data via UDP packets. This is not a system we
plan to export, but it could be nice to design something generic that
could work with that.
