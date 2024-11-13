## Agenda

- Making the solver input the lockdir and the current lockdir a solution cache (@gridbugs)
- Release Dune 3.17.0 and vision for dune 4.0.0 (@maiste)

## Proposal to rename "lockdirs" to "solution caches" and introduce a new "lockfile" which contains the inputs to the solver

The specific function of lockdirs is to cache the packaging solution so that it doesn't need to be computed on each build. On a given machine, the package solution is a pure function of its inputs, so the only reason to cache it is performance; if solving was instant there would be no need for a lockdir. Hence I propose we rename them to "solution caches" to more accurately reflect their purpose to users. Rather than "dune.lock", put it in "dune.solution-cache" or something. The benefit is it would better communicate to users the purpose of the lockdir so they can make a more informed decision about whether to check it into their project, what the consequences are of deleting it are, etc.

There's also an opportunity to better support the workflow of checking in the inputs to the solver (ie. the revision hash of each opam repo and any pinned packages). Currently this information has to go in dune-workspace, but dune-workspace can also contain user-specific configuration (e.g. which warnings to treat as errors) and so it's not always appropriate to check it in. I propose introducing a new file (maybe named "dune.lock") to contain this information.

## Meeting notes

Attendees: @gridbugs @maiste @Leonidas-from-XIV @moyodiallo @MisterDA

### OCamlLSP binary on macOS (@gridbugs)

- It just doesn't seem to work when built with Nix
- Thinks the compiler versions don't match even despite being built with the same compiler
- Check for the magic number, maybe this will shed light on why it doesn't match

### Dune Preview Binary Location (@Leonidas-from-XIV)

- Our current server is very slow from certain locations (very fast in France, acceptably fast from Denmark, extremely slow in Australia)
- Mark put stuff on a different server in the US, we should test
- Worked for @Leonidas-from-XIV, but it didn't work for anyone else, it was just stuck
- Need to talk to Mark about fixing it

### Windows support (@gridbugs)

- @MisterDA is joining to work on Windows
- @gridbugs has tried running stuff on windows
- WinGet works well but getting it to work is hacky

### `@pkg-install` (@maiste)

- Got it working
- Alias is attached to whole project
- Should be up for review later today

### `dune.lock` ignored (@Leonidas-from-XIV)

- Bug report about patches not found when `dune.lock` is ignored
- Should ignoring it be an error?
  * Problematic because `dune.lock` is not a fixed location and the locations for lock files are defined in `dune-workspace` which usually is not committed so it might differ from one user to the other, thus a project would work or not work between different users
  * @gridbugs proposal would eliminate the configurability as it is not necessary anymore to have different directories configurable
- Maybe we should always just ignore the ignore and read the contents anyway?