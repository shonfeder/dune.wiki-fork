## Agenda

- Discussing the upgrade to dune to support `odoc.3` @panglesd
  - See https://github.com/ocaml/dune/issues/11620
- Discussion about the auto-locking @maiste
  - See https://github.com/ocaml/dune/issues/11614

## Meeting notes

Attendees: @panglesd @Leonidas-from-XIV @rgrinberg @alizter @maxim092001 @shym @maiste @rikusilvola "Jason Ho" "Chukwuma Akunyili"

### Upgrade Dune to support odoc 3 (@panglesd)

- Challenges in odoc 3: Mostly new features that the Dune-integration does not enable yet
- Support for hierarchical docs
- Support for assets in hierarchies
- Support for cross-references to other projects including back-references to the current project
- Needs to make the Dune rules aware of the generated hierarchy
- Needs to adjust the install rules
  * @panglesd volunteers to contribute those
- Hierarchy rules are going to be hard because we don't build the docs of dependencies
- Also requires us to install the proper config files for other packages to specify paths etc.
- Can we have a new stanza for the hierarchy definitions?
  * What would the stanza look like?
  * Defines what documents go where in the hierarchy
  * Ok to add but most of the structure should be inferred from the file system
- Can we use the `using` mechanism to opt-into odoc 3?
  * No need to support older odoc and odoc 3, we can just drop support for older odoc
- @panglesd has some code questions but will open a PR and ping people on the specifics

### Autolocking support (@maiste)

- What is the motivation for autolocking? It's useful when developing and adding and removing dependencies
- Need to make sure not to break the project with package updates when the watch mode is enabled
- Solving got a lot faster, so locking won't hold up the process for as long anymore
- The lock directory would need to become a directory target (since we don't know what files will be inside prior to solving so they can't be targets)
- Directory targets are always enabled, the opt-in via `(directory-targets)` is only for user configuration, so we can just use directory targets for solving
- Unclear whether the promotion mechanism can promote a whole directory, but if it doesn't then it can be implemented as part of this and is a useful extension
- Manual locking won't work anymore while watch mode is enabled, but there is no need for it because it will automatically lock when needed
- Updates to opam-repos need to be thought about:
  * Git can just continue to use the same revision, then it is immutable
  * Directory repos can be watched and trigger relocking on changes

### Adding `synopsis` to rules/aliases (@maxim092001)

- Rules can become quite complicated and unclear what they are doing
- It would be good to be able to add some kind of description what they are doing
- Attaching descriptions to aliases can be good, but aliases can be everywhere and the same alias can have multiple rules attached
- `dune describe`/`dune show`
  * It's the same thing, some people liked the word "show" better
- Where to attache the synopsis? On the alias? On the rules?
- How about attaching it to the target? All rules have to have a target (which can be an alias)
- The description can also be on rules, that's ok too
- However having it on the alias can be used to not have to repeat the synopsis on every attached rule
- Why was `action` on `alias` deprecated?
  * Mostly redundant and people were confused what to use, so one approach was axed
- Practical questions: Dune cram tools can run individual tests by `dune build @name-of-test-without-training-.t`
  * No great reason for it, @alizter plans to add support for adding the `.t`
- Practical questions: How to test something in a not-yet-released dune-lang version
  * Dune cram tests can use future dune-lang versions
- `dune describe` might need a recursive flag to show the descriptions defined deeper inside the project hierarchy
- How to deal with aliases defined in every directory (`@fmt`, `@all`): some kind of collecting/deduplicating logic when printing them

### Support for compiler variants (@maiste)

- How difficult would it be to support `ocaml-variants` with toolchains
- The packages just conditionally enable `./configure` flags
- This should work as we implement the `%{}` forms
- Needs to add the variants compiler package to the list of packages to handle specially for toolchains.