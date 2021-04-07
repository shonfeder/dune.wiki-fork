Dune 2.9 will be the last release for the 2.x series. It is meant to be a bugfix and minimal feature set release, such as improvements to RPC to Coq modes.

A tentative release date is mid-May 2021.

### Release process:

2.9 will be almost exclusively consist of _backports_ of bugfixes / PRs merged into `main`, except for the rare cases where the change has not a counterpart for `main`.

In order to get a PR considered for 2.9 you should as PR author:

- submit a PR for `main`, get it merged, add the changelog under the 2.9 section of the `CHANGES` file, target the 2.9 milestone
- once the PR is merged and the milestone confirmed, you can either:
  + if you expect a backport to be "clean" (basically a `git apply`), add the PR to the "Dune 2.9 Backports" project, in the column "To Backport"
    * we will try to backport the PR to 2.9 automatically, in case conflict resolution is too complex we will ask your help to perform the merge
    * once the PR is backported, it will be pushed to a 2.9 CI-staging branch
    * if CI succeeds, then the PR will be pushed to the main 2.9 branch
  + if you expect the PR to have non-trivial conflicts, open an updated PR against the 2.9 branch.
    It is very likely that this is the case due to https://github.com/ocaml/dune/pull/4314 having landed in `main`.

