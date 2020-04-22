## opam file generation

@avsm has been working on the automatic generation of opam files. The
idea is that dune can generate most of what goes in opam files from
its own metadata. For now, the opam files will be promoted to the
source tree and committed. @avsm is planning to finish the first
version of this feature so that we can start experimenting with it.

## Dune fmt

It is now functional and needs to be added to `dune build @fmt`. It
would also be nice to get the same exprience in the editor as with
ocamlformat. It is probably possible to reuse what ocamlformat did.
@emillon is going to ask Josh about it.

## 1.7

Waiting on:

- making virtual libraries using preprocessing work. This will require
  the use of another global hash table (@rgrinberg)
- the c\_executables feature, for ctypes (@rgrinberg)

## Shared artifact cache

@xclerc has a prototype. He will make it available as an experiemental
feature in dune.

For the long term, Tarides will be contracted to develop a more
polished version of this feature, including a daemon that manages the
shared cache.

## Convenience targets

@nojb is working on a feature to make it easier for users to build a
single module in a library. Currently, to achive that one has to know
how dune mangle names, which is tedious.

## Allow to specify the PATH from the workspace file

Currently users have no control over the `PATH` in the various build
contexts. @nojb is working on a feature to allow specifying the `PATH`
in the `dune-workspace` file.

## Small macro system for dune

The idea is to provide a way for users to define new stanzas via OCaml
code.  It is basically an improvement over the current
include/promotion mechanism.  We felt the need for this feature in
several cases.  The design space for this feature is quite
large. @diml is going to write a draft RFC so that we can brainstorm.

## Memoization API

Work is in progress to integrate it more in Dune, so that we can rely
on it more.

## Dune upgrade

This is ready in master. @diml is going to write a post about it.

## Rotor integration

@diml is working on a `dune rotor` command, that will call `rotor`
with all the right options. This will make it much easier to use rotor
on dune workspaces.
