Dune 2.9 will be the last release for the 2.x series. It is meant to be a bugfix and minimal feature set release, such as improvements to editor support or Coq build rules.

The release schedule is as follows:
- June 11th 2021: last day to submit PRs targeting the 2.9 milestone
- June 18th 2021: release day

2.9 will be almost exclusively consist of _backports_ of bugfixes / PRs already merged into `main`, except for the rare cases where the change has not a counterpart for `main`.

### Pull Request workflow:

We will track pull requests scheduled for 2.9 using a Github Project, in particular: https://github.com/ocaml/dune/projects/3

Columns there are meant to track the lifetime of a PR, from its inclusion in main, to its release in 2.9.

In order to get a PR included in 2.9 please:

- submit a PR for `main`, but milestone `2.9`, and add it to the column "Pending Approval". The changelog should be added under the 2.9 section of the `CHANGES` file.
- once the PR is merged in `main`, it will be moved to the "Pending Backport Column", please prepare a backported PR , but targeting the 2.9 branch.
- once the backport PR is submitted, add the backport PR to the "Pending CI" column.
- once CI is ready, the 2.9 Release Managers will merge the PR.

## Things to check before the release:

- Changes section should be coherent between `main` and `2.9`