# Agenda

* Lockdir target changes to tests (@Leonidas-from-XIV)
* MSP (or minimal opam parity) milestone (@shonfeder): https://github.com/ocaml/dune/milestone/62
* `with-dev-setup` (@shonfeder)

# Meeting notes

Attendees: @aguluman, @rgrinberg (first part), @Alizter, @Sudha247, @shonfeder, @Leonidas-from-XIV

## Lockdir target changes to tests (@Leonidas-from-XIV)

* With the new lock dirs as targets the test can't write to the lock dir anymore because these changes are done by Dune
* This affects a lot of tests that construct lock dirs manually
* This can be changed by converting the lock dir entries to OPAM files and running the solver to make Dune build them
* We don't want to run the solver
* We probably don't want to replace the tests with ones that use the solver because that is a lot of tests
* Set up copy rules to copy a possibly existing lock dir into the private context from which the lock dir is read
* We are confident that we want to make Dune generate the lock files automatically
* But we probably don't want to drop support for lock dirs in the source directory just yet

## Dune MSP (@shonfeder)

* Was a previous Opam parity milestone
* There is a new milestone
* Non-Tarides contributors don't use the milestones, so Tarides is free to manage the milestones as they see fit
* Idea is to allow an OPAM-equivalent workflow
  - Have a working compiler
  - Able to use dev tools
* Try to keep the scope small
* Should we have support for system-wide installation?
  - A global installation is an option that a lot of generic package managers (Nix, OPAM) support
  - System-wide binaries are simple-ish, but system-wide libraries are tricky
  - Currently things can be done with a dummy project, lots of Dune devs do this
  - It's fairly tedious to set up these dummy projects, one could think of Dune automatically creating temporary dummy projects on demand

## `:with-dev-setup` (@shonfeder)

* We currently mostly ignore `:with-dev-setup`
* Dev tools are handled through a different mechanism but probably shouldn't
* Dune could handle the tools it knows about in `:with-dev-setup` differently while handling the rest generically
* The current state is annoying if you want to install tools outside of the blessed list
* Tools that Dune knows about is hardcoded and rapidly growing
* It grew organically as demand for it appeared but not in a consistent manner
* Maybe we need to spec out the description how dev-tools should work and then implement that
* Some tools are tricky because we need to match the compiler that the project uses

## Scope of Dune (@shonfeder)

* What should be the goal of Dune package management?
  - Should we replace all OPAM usecases?
* Who is the target audience of the OCaml platform?
  - Developers yes
  - Users that want to install packages that are written in OCaml?
* Possibly requires user research
* See [this comment](https://github.com/ocaml/dune/issues/12107#issuecomment-3161771786) for more information