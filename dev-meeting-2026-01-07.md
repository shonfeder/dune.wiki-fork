## Agenda
 * Move to Zulip (@Leonidas-from-XIV)
 * Pinning git repos should record revision https://github.com/ocaml/dune/issues/13110 (@Leonidas-from-XIV)

Attendees: Ali, Ambre, Chukwuma, Marek, Puneeth, Rudi, Shon, Sudha

## Notes
- Move to Zulip (@Leonidas-from-XIV)
  - Everyone agrees
  - Let's announce:
    - Reiterate in slack
    - Update slack channel description
    - Announce in discuss?
- Pinning git repos should record revision https://github.com/ocaml/dune/issues/13110 (@Leonidas-from-XIV)
  - Semantics:
    - currently adding pins to a branch make the lockfile non-reproducible
    - should lock-file mean all and only reproducible?
    - 
  - Rudi:
    - For a point of comparison, look at how we handle tarballs without a checksum
  - Marek: implication
  - What about sources that come from a local file system?
    - Rudi:
      - let's look at these analogous to
    - Editable vs. non-editable pins of local artifacts
      - Pinning to file path, then it is "editable"
      - Pinning use git file locally, then it is 
        - Issue a warning if a git-pinned local dir is dirty
  - Shon: mentions 
- Ongoing release (@shonfeder)
  - Still hunting regressions
    - Currently have ~1200 excess revdep failures
  - Still expensive to do manually
  - Shon: we have been planning to focus on improving release cadence
  - Ali: has WIP PR to run revdeps in nix
    - This can help
- https://github.com/ocaml/dune/issues/13225
  - Should we support `:standard` inside an `:include`?
  - Conclusion, yes.
- https://github.com/ocaml/dune/issues/13202
  - Help improving test perf of expensive tests welcome

## Action items 

- Announce move of dune-dev meetings to Zulip
- Update documentation
- Issue a warning if a git-pinned local dir is dirty
