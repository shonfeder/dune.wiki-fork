* Autolocking Dependency update model (Sudha)
  * Problem ([#13011](https://github.com/ocaml/dune/issues/13011)): When Auto-locking is enabled, Dune commands like dune build, dune runtest , etc. silently re-lock dependencies against upstream opam-repository. This could lead to consequences like compiler updates when one is just running tests. It makes local builds depend on the mutable opam-repo universe. This has been reported by some users like @samoht.
  * Proposed solutions: After several discussions, there are several proposed solutions.
  1. Change autolocking update model: Update the dependency solution only when user requests for it, or dependencies change.
  2. Introduce a new auto-locking mode: Preserve the current auto-locking mode as lockless mode, and introduce an auto-locking mode where the first build generates lockfiles in the source tree.
  3. Configurable lazy or eager updates: Make it possible for users to choose whether they want eager updates (every time dune build runs) or lazy (when they request, or dependencies change)
  4. Update only in specific intervals: Don't update from opam-repo on every build, do it in an interval like one day or two days, etc.  
  5. Updates running in the background: Without interrupting user commands, updates can happen in the background.