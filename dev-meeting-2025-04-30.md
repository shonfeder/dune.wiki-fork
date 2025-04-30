## Agenda

## Meeting notes

Attendees: @gridbugs @maiste @shym @rikusilvola @Leonidas-from-XIV Jason Ho

### How toolchains evaluate directories (@maiste)

- Previous version of OxCaml could be compiled using @dra27's patches
- New version of OxCaml changed something, which causes the configure script to not be found anymore
- Is there a repro-case? Maybe put it in the hello-oxcaml repository
- @gridbugs will take a look but toolchains does not intercept calls to `configure` usually

### Release 3.18.2 (@maiste)

- OCaml 5.4 added a new binding in `Sig` which shadowed an existing binding
- @nojb fixed it on `main` a while ago, @kit-ty-kate asked whether 3.18 can be patched
- @Leonidas-from-XIV backported the patch, @maiste released 3.18.2 with it
- It's on opam-repository, we expect it to be merged soon

### Dune build & RPC (@gridbugs)

- `dune build` instead of failing about an existing lock will send build requests to the RPC server
- All the commands that build under the hood (`runtest`, `exec`) are slightly different so it will need to be implemented for each one of them
- `dune promote` is a challenge, since the variable for enabling auto-promotion does not seem to work consistently
- The lock in the build directory is actually not locking the build but rather locking starting another RPC server. That's probably not right, because multiple builds can happen in parallel as long as there is no RPC server. That's probably unsafe
- Other approach could be to wait on the lock while it is held by another process
- However this is problematic with watch mode, as that would block forever
- Watch mode would need to release the lock after every build iteration
- Would it make sense to make the locks finer grained?
  * Possibly, but we already have a global lock and adding more would be inefficient and possibly cause bugs
- Currently the build configuration (targets, flags etc) is inherited from the original launching command, so a command building a different target can't be transmitted

### Portable lock files (@gridbugs)

- There is a PR
- It would be nice to have it merged soon to prevent it from bitrotting
- Rudi wants to deduplicate package lists and build instructions wherever possible to make the lock file less verbose
- The set of platforms for which the lock file is generated is hardcoded and can't be configured
- Locking takes about a second per platform, but we still have to prioritize which platforms to include by default
- Could be made smarter in the future about distributions
- There is no way to generate a solution that
  * Is tested on all platforms
  * Is up to date on all platforms
  * At the same time. Choose 1. We chose option 2
- Depexts are currently not handled in a portable way but that isn't complicated
- We could use a project wide `available` field, where we could exclude platforms that we explicitely don't support.
- Where should the configuration which platforms to use go?
  * `dune-project` is committed, which has the advantage that different developers would generate the same lock file. But it seems a bit strange of a place to configure it.
  * `dune-workspace` already contains lock dir configuration but different users might have different workspace configurations.