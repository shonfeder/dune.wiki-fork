Attendees: @gridbugs, @Leonidas-from-XIV, @rgrinberg, @maiste, @ManasJayanth

# Compile-time toggling of feature flags

 - unifying the `DUNE_CONFIG__` variables with compile-time configuration by setting the default values of config variables at configure-time which can still be overridden at runtime (by setting config variables).
 - it's important to keep the `--help` messages of the `configure` script up to date with documentation about new flags. The configure script has no dependencies so doing this in automated way might be hard.
 - coordinating changes to dune_config/config.ml between Jane Street and Tarides. Jane Street is trying to make this easier by extending the engine so future changes don't require modifying it directly. Changes to the engine and its dependencies (cache, rpc) currently requires going through an RFC process.

# Dune Dev Preview

 - lots of positive responses to sharing a demo showing off some new features on social media

# Workshop at Jane Street in November

 - a couple of dune developers will work with Jane Street to upstream some of the new features of dune from their internal fork
 - most features are improvements to stability and performance, not user-facing

# Auto-relocking

 - this is a proposed feature where dune would rerun the solver and re-generate the lock directory when the dependencies of a project change when running `dune build` and `dune build --watch`. Dune is currently able to detect when the dependencies described in dune-project don't match what's recorded in the lockdir but there are some complications relating to when and how to update the lockdir.
 - the main issue is that there is no easy way for auto-relocking to work in watch-mode. The contract of watch mode is that it rebuilds _targets_ when one of their dependencies (in the sense of source files, not packages) changes. Lockdirs are not targets, and so watch mode is unable to build them. One possibility is to make lockdirs into targets and solving into an action. This would allow relocking to be fulfilled by the contract of watch mode but would be non-trivial.
 - there's another issue with users' expectations about under which conditions the lockdir is updated. Currently the only command that updates the lockdir is `dune pkg lock`, and this has the effect of both updating the local copy of the opam repo and any pinned packages, _and_ solving the dependencies and updating the lockdir. If this happened on `dune build` after a project's dependencies change, then changing the deps of a project and building it will have the (probably) unintended side effect of updating the version of all dependencies of the project, which somewhat defeats the point of having a lockdir to begin with.
   - comparing to how npm solves this problem, in npm lockfiles are only updated by the `npm install` command when the specified dependencies don't match the lockfile (or the lockfile is missing). Unlike dune, npm doesn't have a notion of building packages built into it so there's no option for updating the lockfile on builds, and it also doesn't have to deal with watch mode itself (though some external tools provide a watch mode).
   - comparing to cargo, in cargo the lockfile is updated when running `cargo build` after the dependencies change. It's not clear to any attendees who have used cargo to what extend `cargo build` is allowed to bump version of locked dependencies to satisfy any new dependencies added since the last build. It doesn't have semantics that are clear to any of us who use cargo and it's not necessarily something we want to emulate in dune. Cargo has a watch mode but it just runs `cargo build` when files change
 - It was proposed that dune could print a message when the dependencies change, rather than attempting to automatically relock the project. This would avoid the technical issue with supporting watch mode and avoid the semantic issue of dealing with the possibility that versions of locked dependencies may be unintentionally bumped by building the project. The proposed solution would have dune watch mode pause asking the user to run `dune pkg lock` when it detected that there are dependencies in dune-project that are missing from the lockdir. In non-watch mode `dune build` would exit with a message asking the user to run `dune pkg lock`.

# Package Progress Messages

 - Dune recently got a feature that prints messages to the console as packages are downloaded and built, currently enabled by a config variable.
 - In its current implementation it unconditionally prints messages despite the user's configuration for how much output dune should display.
 - The default setting for this is "quiet" where dune will only print a single dynamically-updated status line, but the current implementation of package progress messages ignores this setting and prints something resembling what dune would typically print in "short" mode instead.
 - It was proposed to update package progress messages to respect the display setting and only keep its current behaviour in short mode.
 - In "quiet" mode it would be ok to print package progress updates to the status line instead, in a similar style to opam or nix.