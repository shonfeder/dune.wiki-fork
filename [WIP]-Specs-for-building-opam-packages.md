The goal of this ticket is to design and explain how we're going to build and install opam packages in dune. This is part of the project of turning dune into a full blown package manager capable of replacing opam.

We assume we've already obtained package sources and their build steps after constructing the lock file.

To build a package, we need to construct a single rule that will produce a directory target that contains all the artifacts in the package. Like all other rules, it will have dependencies, targets, and an action.

### Target

The target will be a directory with all the installed artifacts of the package

### Dependencies

The dependencies will be:

1. The sources of the package being built.
2. For every single package dependency, the target directory with all the build artifacts will be added as a dependency

### Action

The action will produce the directory with the build artifacts by executing the following steps:

1. Apply whatever source patches
2. Set `PATH` and `OCAMLPATH` to include all the bin/ and lib/ directories of all the dependency packages 
3. Run the command in the `build` step of the opam file.
4. If there's no `install` field, read the .install` it and move all the files to the directory target.
5. If there's an `install` field, we run it with the destination directory being our directory target.

## Future Development

For v1, we will not implement:

1. Sandboxing of the build/installation steps
2. depexts