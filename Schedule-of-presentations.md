Presentation are on Wednesdays (alternating with Dune meetings), 16:00 UK time.

## 2022 (reverse chronological order)

- 28/09/2022: Makarius Wenzel: "Isabelle as a build tool for Isabelle"
  + Jitsi link: https://rendez-vous.renater.fr/isa-build
  + Slides: 
  > Isabelle as build tool for Isabelle
  >
  > The tooling for Isabelle is always Isabelle itself. The system has a
  > mathematical world: Isabelle/ML for functional programming and
  > symbolic logic; its applications are called "sessions" and either
  > built implicitly on demand or explicitly via "isabelle build" on the
  > command-line. The result is a heap file and a session database (SQLite
  > or PostGreSQL), it includes HTML and LaTeX presentation information or
  > other export artifacts.
  >
  > Then, there is a physical world: Isabelle/Scala for system management,
  > server connections, and interaction with the user via the Prover IDE,
  > e.g. Isabelle/jEdit or Isabelle/VSCode. System components are built
  > implicitly on demand or explicitly via "isabelle scala_build". That is
  > based on pure Java, to simplify the bootstrap process of
  > everything. Its own build process is managed by a plain shell script,
  > without fancy (complex, unstable) build tools for Java.
- 16/03/2022: Charlie Curtsinger and Daniel Barowy (authors of [LaForge](https://arxiv.org/abs/2108.12469)) will give a talk "Riker: Don't make, Make it so"
- 02/02/2022: François Bobot: `implicit_transitive_deps` past and possible futurs

## 2021
- 20/01/2021: Rudi Grinberg will present his work on adding a RPC to Dune
- 03/02/2021: Ulysse Gérard will present the custom-build-info feature
- 17/02/2021: Etienne Millon will present yarn and jest
- 03/03/2021:
- 17/03/2021: Rudi Grinberg will present pulp, spago, psc-package (PureScript's toolchain)
- 31/03/2021: Jon Poole will present the https://please.build/ build system
- 14/04/2021: Lubega Simon will present his work on odoc and Dune
- 12/05/2021: Thibaut Mattio will present spin https://github.com/tmattio/spin, an OCaml project generator
- 26/05/2021: Jeremie Dimino will present how to use Dune's new and shiny cores' API
- 09/06/2021: Gargi Sharma and Rizo Isrof will present [current-bench](https://github.com/ocurrent/current-bench), the system we use to benchmark Dune
- 01/09/2021: Rudi Grinberg will present the Dune plugin for VSCode
- 13/10/2021: Andrey Mokhov will give a talk about some cool aspects of Memo's implementation, including the new faster cycle detection algorithm
- 24/11/2021: All and Rudi Grinberg, Discussion: How can we dynamically add new targets? How can we gather the errors of dependencies of a target?
- 24/11/2021: François Bobot, Discussion: The different steps of the parsing of Dune files ( Sub-dir, data-dirs, dune generated files).

(Idea of discussion?): What feature dune needs from the compiler (i.e https://github.com/ocaml/dune/wiki/OCamlFeatureRequest)