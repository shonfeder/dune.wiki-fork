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
clear direction, and some part are kind of out of control. This is
unlikely to change in the short/medium term, but we should think about
giving a clear semantic to the language in the long term. This might
require deeply changing some aspects of the language.

Since we were talking about the dune language, the JSON topic was
brought up as expected. JSON as become a standard and using JSON would
mean that we would benefit from existing tooling built around it.
Jérémie is planning to generalise the Dune's parsing engine, which
should give us a JSON syntax for free. However, the syntax of the
action DSL will be pretty ugly. Variables might also be a bit messy.

To be continued.

## Dune fmt

We agreed that the pretty-printing of dune files should be versionned
just like many other aspect of Dune. This mean that if one day we
change the formatting of dune files, we will need to keep support for
the old versionning.
