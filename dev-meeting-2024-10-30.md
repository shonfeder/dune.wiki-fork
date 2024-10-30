# Agenda

- Proposal to rename "lockdirs" to "solution caches" and introduce a new "lockfile" which contains the inputs to the solver (cf below)
- Is there a way to define a true alias without having to insert it in a rule?
- Release 3.16.1

## Proposal to rename "lockdirs" to "solution caches" and introduce a new "lockfile" which contains the inputs to the solver

The specific function of lockdirs is to cache the packaging solution so that it doesn't need to be computed on each build. On a given machine, the package solution is a pure function of its inputs, so the only reason to cache it is performance; if solving was instant there would be no need for a lockdir. Hence I propose we rename them to "solution caches" to more accurately reflect their purpose to users. Rather than "dune.lock", put it in "dune.solution-cache" or something. The benefit is it would better communicate to users the purpose of the lockdir so they can make a more informed decision about whether to check it into their project, what the consequences are of deleting it are, etc.

There's also an opportunity to better support the workflow of checking in the inputs to the solver (ie. the revision hash of each opam repo and any pinned packages). Currently this information has to go in dune-workspace, but dune-workspace can also contain user-specific configuration (e.g. which warnings to treat as errors) and so it's not always appropriate to check it in. I propose introducing a new file (maybe named "dune.lock") to contain this information.