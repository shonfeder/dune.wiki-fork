# Agenda

- https://github.com/ocaml/dune/pull/14318 (@Leonidas-from-XIV)

# Meeting notes

## `DUNE_PKG`

- The code and tests in the PR are fine
- The functionality is already available via CLI flag
- Do we have user demand for it?
- At the moment it is only @shonfeder pushing for it
- We already ignore some env flags that other build systems pass like CFLAGS
- There's a good reason why we don't accept env variables like CFLAGS
  * Otherwise all rules would need to depend on CFLAGS
- Submitter needs to better motivate the feature

## Odoc rules

- Jon and Arthur want to break backward compatibility with older odoc versions
- No concerns from Rudi
- We never versioned Odoc rules, no need to start doing that now

## Robin's test PRs

- There are a number of PRs awaiting review
- Robin has merge right, he can merge them
- No big issue if the tests aren't perfect
- Robin will go on a merge spree and merge them