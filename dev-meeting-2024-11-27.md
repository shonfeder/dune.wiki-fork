## Agenda

- Point on the `dune.3.17.0` release
- Adding a `depends` alias to `libraries` for consistency. Discussing the conventions in the naming.

## Meeting notes

Attendees:  @maiste @Leonidas-from-XIV @ElectreAAS

### Dune.3.17.0 release (@ElectreAAS, @maiste)

- Revert one change that was breaking formatting to be included in future release. The change may be reverted from `main` and rework later.
- Not major issues detected.

### Adding a `depend` alias for `libraries` (@maiste)

- Introduce consistency because it can confuse the user to have  `depend` in `dune-project` and a `libraries` in `dune` file.
- It adds overhead and confusion to have the alias.
- Replacing it would break more confusion for established users.
- The meaning within the `dune-project` file and the `dune` file is different, so it would make sense to do the change. 

### Future work for Package Management (all)

- Get read of the lock files
- Automatic locking in watch mode:
  - Needs to get watching on lock files
  - Potential breaking in development as we need to rerun the solver. 
  - Need to be configurable in a config file `.config/dune` and `workspace`
- Add automatic library:
  - When adding a `libraries` it should update the `depend` in the `dune-project`.
  - It comes with a list of issue to solve:
   1. Where does the information live, and who does introduce the value in `opam` file?
   2. We don't know if a library is available until it is installed (with optional, for example)?
   3. Who would ensure the database is maintained and correct?
   4. What about conflict in library declarations?

