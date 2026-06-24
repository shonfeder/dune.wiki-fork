
- Filter inter-lib deps (Ali)
  - Comment from Gabriel regarding compiler side: https://github.com/ocaml/dune/pull/14492#issuecomment-4783085542
  - Question of soundness of builds due to `-I` flags only specifying directly, and letting unintended `.cmi`s getting looped into the build
  - By reducing the overage of the over approximation, we are getting out of sync with what the compiler can declare at the CLI (there's discussion of how this can be fixed via a new compiler flag like `-i-manifest` to improve precision of what's in scope)
  - OxCaml includes compile-time metaprogramming, which may trigger the issue Gabriel is mentioning
  - But we have not been able to demonstrate breakage yet, and dev teams is OK with moving forward with the PR.
  - Also produces observable slowdown in all builds, but we have not yet seen a case where the benefits yield speedup
    - This would be more compelling if we had clear evidence of the speedup/improvements
    - We have at least reports of it producing improvements on some code basis
    - We could demonstrate this on dune if we can find a way to see thru "import modules", seems technically feasible

- Path representation change for the 3.24 release (Shon)
  - [#dune > new path representation in 3.24](https://ocaml.zulipchat.com/#narrow/channel/527834-dune/topic/new.20path.20representation.20in.203.2E24/with/606132373)
  - All dev team agrees that the new behavior is correct, and problematic usage was wrong or due to brittle tests
  - We will advance with this change, propose upper bounds, and leave it to packages to fix their behavior

- Absolute dirs on cli (Ali)
  - re: https://github.com/ocaml/dune/issues/15105, https://github.com/ocaml/dune/issues/12230
  - what's responsible for relativizing: RPC server or client?
  - Rudi: RPC protocol should only received paths relative to workspace root
  - Devs decided to fix each individual case and then maybe do a refactor
  - Suggestion to separate problem of crashes from problem of adding full support for absolute path, but was dismissed