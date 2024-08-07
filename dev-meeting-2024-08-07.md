Present: @Leonidas-from-XIV @leostera @nojb @maxRN Lucas M.

## Flags for non-standard behavior @Leonidas-from-XIV

- We plan to introduce some mechanisms that are opt-in because they're not compatible with opam workflows
- For this we want to have extra configuration flags
- The idea is that they use the existing Dune Config mechanism but the defaults would be changed at build-time
- Draft PR: [#10724](https://github.com/ocaml/dune/pull/10724)
- @nojb: Can't this be configured by Dune Workspace?
- @leostera: The idea is that this would prepare for an opam-less workflow where users download a Dune binary and that will have all the configuration out of the box.

## Package Coverage @Leonidas-from-XIV

- We are running a tool that checks how many projects we can currently build out of the box without OPAM
- Initial run was 19% though by fixing the two most common failure reasons (`seq` and `either`) we are at about 53% at time of writing
- Lucas M: Will Dune support building projects not using Dune?
- @Leonidas-from-XIV: Not directly, but other projects that don't use Dune are supposed to be usable. Checking that we can depend on every OPAM package is a slightly different question but the idea is that users should be able to depend on any opam package.

## `dune-project` formatting @maxRN

- @maxRN started working on this, the dune formatter currently only autoformats `dune` files not `dune-project` files. Formatting the latter is a bit trickier since long strings (`description`, `synopsis`) get messed up
- @maxRN is currently adding support for the parser to know whether the input was a long-string or not
- @Leonidas-from-XIV: Another option would be to have the pretty-printer decide how to format a string on some rules (length, newlines etc)
- @Leonidas-from-XIV: This will come in very handy if we want to programmatically write to `dune-project` as then it can get reformatted nicely!

## Q&A

- Lucas M: Will we deprecate opam file generation, now that Dune is not depending on OPAM much?
  * @Leonidas-from-XIV: We currently can read dependencies from `dune-project` and `opam` files so that will not go away any time soon, removing it would be a major release bump
  * @leostera: OPAM files and the opam-repository are still underpinning everything, we are mostly just making an alternative to `opam` the binary
- Lucas M: Is there a way to have `build --watch` and `exec` work?
  * @Leonidas-from-XIV: Not at the moment, its not quite trivial to have two dune processes run at the same time. In theory they could communicate through RPC but its not quite simple
  * @maxRN: There is [#9290](https://github.com/ocaml/dune/issues/9290) for tracking this
  * @Leonidas-from-XIV (somewhat later): It is possible to run `exec` in watch mode however, as a workaround
- @nojb: Will Dune Pkg Mgmt support injecting external libraries into the dependencies via `OCAMLPATH`
  * @Leonidas-from-XIV: Technically it should be doable, we just haven't tested it
  * @leostera: This is not something we focus on at the moment, to prioritize doing everything though Dune
  * @Leonidas-from-XIV: In any case I believe you can write an `opam` file and `.install` script to let Dune Pkg Mgmt know about the files, then these would work as any other opam package and should be supported
- @nojb: Is supporting environment supplied compilers possible in Dune Pkg Mgmt? Or how do compilers work?
  * @Leonidas-from-XIV: Dune Pkg Mgmt selects a fitting compiler and builds it as any other opam dependency, but external compilers work if using the `ocaml-system` package