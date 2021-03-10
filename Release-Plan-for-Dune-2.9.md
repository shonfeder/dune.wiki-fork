Dune 2.9 will be the last release for the 2.x series. It is meant to be a bugfix and minimal feature set release, such as improvements to RPC to Coq modes.

Release should happen April-May 2021.

### Inclusion process

2.9 will be almost exclusively consist of _backports_ of bugfixes / PRs merged into `main`, except for the rare cases where the change has not a counterpart for `main`; in order to get a PR considered for 2.9 you should as PR author:

- submit a PR for `main`, get it merged, add the changelog to 2.9
- once your PR is merged, add it to the "Dune 2.9 Backports" project, in the column "To Backport"
- we will try to backport the PR to 2.9 automatically, in case conflict resolution is too complex we will ask your help to perform the merge
- once the PR is backported, it will be pushed to a 2.9 CI-staging branch
- if CI succeeds, then the PR will be pushed to the main 2.9 branch

