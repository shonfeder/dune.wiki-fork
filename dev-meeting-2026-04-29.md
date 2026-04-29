# Agenda

- https://github.com/ocaml/dune/pull/14318 (@Leonidas-from-XIV)

# Meeting notes

Attendees: @Alizter @robinbb @Leonidas-from-XIV @ElectreAAs @rgrinberg Chukwuma Akunyili @art-w

## `DUNE_PKG` (@Leonidas-from-XIV)

- The code and tests in the PR are fine
- The functionality is already available via CLI flag
- Do we have user demand for it?
- At the moment it is only @shonfeder pushing for it
- We already ignore some env flags that other build systems pass like CFLAGS
- There's a good reason why we don't accept env variables like CFLAGS
  * Otherwise all rules would need to depend on CFLAGS
- Submitter needs to better motivate the feature

## Odoc rules (@art-w)

- Jon and Arthur want to break backward compatibility with older odoc versions
- No concerns from Rudi
- We never versioned Odoc rules, no need to start doing that now

## Robin's test PRs (@robinbb)

- There are a number of PRs awaiting review
- Robin has merge right, he can merge them
- No big issue if the tests aren't perfect
- Robin will go on a merge spree and merge them

## Robin's dependency PR (@robinbb)

- Will this speed up clean builds?
- Dune codebase defeats this optimization, not a good place to test it
- Cohttp is a better candidate
- No need for synthethic benchmarks
  * Real world builds is what matters
- OCaml builds tend to be very serial
  * Rarely using all cores anyway
  * This gives more potential for parallelization
- Can the PR be shrunk by removing some optimization?
  * Unwrapped libraries optimization might remove 100 lines
  * Unclear whether the unoptimized implementation will be any shorter
  * Optimizations are there because some tests were failing
  * Robin will take a look
- Runtime deps from PPX will need to be always added
  * `ppx_deriving` is an example of runtime deps in the wild
  * `ocamldep` runs before PPX, so runtime deps are invisible to `ocamldep`
  * should be easy to construct a repro case
  * we could run `ocamldep` later, but nobody wants to do this
- Codept might help us but unclear how well it is maintained at the moment
- Can we have the new code in parallel with the old?
  * We could gate it behind a flag and optimize it in the background
  * The 20% penalty is acceptable
  * The closure calculation can be optimized
  * We always calculate it from scratch, even if subclosures were already calculated
  * This optimization is nice, after the PR it becomes even more useful to do
  * Jane Street has done it internally and it was a huge win
  