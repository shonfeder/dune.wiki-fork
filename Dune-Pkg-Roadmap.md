**Contents**

<!-- markdown-toc start - Don't edit this section. Run M-x markdown-toc-refresh-toc -->
**Table of Contents**

- [Target](#target)
- [Roadmap](#roadmap)
  - [Phase I: Prototyping **completed**](#phase-i-prototyping-completed)
  - [Phase II: Stabilisation **active**](#phase-ii-stabilisation-active)
    - [1. Improve Release Process and CI Coverage **in progress**](#1-improve-release-process-and-ci-coverage-in-progress)
    - [2. Relocatable compiler support **completed**](#2-relocatable-compiler-support-completed)
    - [3. Support for external packages depending on locally defined packages **in progress**](#3-support-for-external-packages-depending-on-locally-defined-packages-in-progress)
    - [4. Mature Support for Development tools **in progress**](#4-mature-support-for-development-tools-in-progress)
  - [Phase III: Integration **planned**](#phase-iii-integration-planned)
    - [1. Inclusion in the opam-ci  **TODO**](#1-inclusion-in-the-opam-ci--todo)
    - [2. Windows Support **TODO**](#2-windows-support-todo)
    - [3. Mature the binary distribution and installation documentation **TODO**](#3-mature-the-binary-distribution-and-installation-documentation-todo)
    - [4. First-class Cross-compilation Support **TODO**](#4-first-class-cross-compilation-support-todo)
    - [5. First-class OxCaml Support **TODO**](#5-first-class-oxcaml-support-todo)
    - [6. First-class vendoring **TODO**](#6-first-class-vendoring-todo)

<!-- markdown-toc end -->

# Target

**Why:** The tooling used for developing OCaml software can be made simpler and more efficient. These improvements can benefit all OCaml software development flows, whether driven by humans or AI agents, and will help remove barriers to newcomers.

**What:** Our goal with dune package management is *to simplify, and improve the efficiency of, developing OCaml projects.*

**How:** We are doing this mainly by approaching the package management problem
as a build problem, and developing tooling for managing key parts of the OCaml SDLC on this basis. This functionality should be elegant, performant, principled, stable, understandable, flexible, and available on all [Tier-1 platforms](https://github.com/ocaml/ocaml?tab=readme-ov-file#overview).

# Roadmap

The following milestones lay out our high-level plans, separated into three phases of development. The milestones are sequenced in dependency order, so that earlier developments are expected to make the subsequent ones easier to deliver and secured with more robust foundations.

## Phase I: Prototyping **completed**

The initial prototype of dune package management was properly inaugurated in [\[RFC\] Dune Package Management · Issue #7680 · ocaml/dune](https://github.com/ocaml/dune/issues/7680). Nearly all of the milestones in the following phases are drawn from the initial RFC, and substantial segments of the core functionality has been completed, as reported in [Dune Package Management Updates \- Ecosystem](https://discuss.ocaml.org/t/dune-package-management-updates/18023). Our testing has shown the features are not yet mature enough to recommend wider use, beyond eager and early adopters. Consequently, all the documentation warns that this functionality is experimental and subject to change.

## Phase II: Stabilisation **active**

In the stabilisation phase, our focus is on maturing the experimental functionality into a stable, mature tool for mainstream OCaml development on MacOS and Linux.

### 1. Improve Release Process and CI Coverage **in progress**

**Why this matters:** Faster feedback cycles will help drive development of robust and user-centric tooling, and maturity in our release and integration processes will drive greater stability and better flow in our development.
**Target completion**: Q2 of 2026
**Tracked by:** [Improve release automation and reliability · Issue #13771 · ocaml/dune](https://github.com/ocaml/dune/issues/13771)

### 2. Relocatable compiler support **completed**

**Why this matters:** David Allsopp’s [recently completed solution for a relocatable OCaml compiler](https://discuss.ocaml.org/t/relocatable-ocaml/17253) simplifies management of the compiler toolchain considerably. Supporting the relocatable compiler in Dune enables us to build the compiler like a regular package, making it easy to cache and share compilers, speeding up project initialization when using cached compilers and unlocking a simplification of the dune packagement management architecture.
**Target completion**: Q2 of 2026
**Tracked by:** [Relocatable compiler in dune · Issue #13229 · ocaml/dune](https://github.com/ocaml/dune/issues/13229)

### 3. Support for external packages depending on locally defined packages **in progress**

**Why this matters:** Lack of support for this common dependency pattern prevents the use of dune package management for developing many important projects in the OCaml ecosystem, such as Lwt *and Dune itself*\! To illustrate, Dune’s own external test dependencies transitively depend on `dune-configurator`. However, `dune-configurator` is part of the Dune source tree, and dune package management does not support this direction of dependency.

This problem does not prevent *using* such packages as dependencies (e.g. dune package management works fine on projects that depend on Lwt), but it does prevent us from using Dune package management for managing packages for such projects dogfooding dune package management ,  depriving us of an important feedback loop in the development process.
**Target completion**: Q3 of 2026
**Tracked by:** [Overlapping Dependencies in Lock Directory ("in and out problem") · Issue #8652 · ocaml/dune](https://github.com/ocaml/dune/issues/8652)

### 4. Mature Support for Development tools **in progress**

**Why this matters:** Software development makes use of a variety of tools. Standard tools include REPLs, formatters, and LSP servers, but one of the benefits of working in OCaml is that we get early access to a wealth of unique tooling written in OCaml and distributed through the opam repository. Dune currently provides limited support for a hard-coded subset of standard development tools, but  current implementation is brittle and requires a principled redesign to enable stable, extensible support for all the OCaml tools an OCaml dev may want to use.
**Target completion**: Q4 of 2026
**Tracked by:** [Redesign and reimplement support for dev tools · Issue #12914 · ocaml/dune](https://github.com/ocaml/dune/issues/12914)

## Phase III: Integration **planned**

In the integration phase, we will aim to extend the breadth of support to all Tier-1 platforms, and ensure that dune package management is fully integrated in the OCaml package ecosystem as a co-equal peer with opam.

### 1. Inclusion in the opam-ci  **TODO**

**Why this matters:** The opam-ci runs ecosystem-level integration tests on all packages published to the opam repository. For dune to be a peer of opam in the package management space, it is necessary that it achieve comparable stability, which will involve some level of testing.
**Target completion**: Q4 of 2026
**Tracked by:** [Windows support for package management · Issue #11161 · ocaml/dune](https://github.com/ocaml/dune/issues/11161)

### 2. Windows Support **TODO**

**Why this matters:** Windows is a Tier-1 platform for the OCaml compiler and is used by most developers worldwide.
**Target completion**: Q1 of 2027
**Tracked by:** [Windows support for package management · Issue #11161 · ocaml/dune](https://github.com/ocaml/dune/issues/11161)

### 3. Mature the binary distribution and installation documentation **TODO**

**Why this matters:** For those who wish to adopt Dune’s approach to package management, it becomes possible to develop OCaml projects without needing opam. It therefore becomes useful to have a way of installing dune easily without needing to depend on an opam installation. The [dune binary distribution](https://nightly.dune.build/) has been instrumental in enabling rapid feedback from early adopters of dune package management and it is used by [setup-dune](https://github.com/ocaml-dune/setup-dune). While this has sufficed in its current state for the experimental stage of dune package managementt, as dune package management moves to integrate with the broader ecosystem, we will need to harden the distribution, and we will eventually seek to move documentation and hosting of the distribution to align with [ocaml.org](http://ocaml.org) as a complementary alternative to an opam based setup.
**Target completion**: Q1 of 2027
**Tracked by:** [Mature the binary distribution • Issue #236 • ocaml-dune/binary-distribution](https://github.com/ocaml-dune/binary-distribution/issues/236)

### 4. First-class Cross-compilation Support **TODO**

**Why this matters:** Improved cross compilation offers benefits for resource constrained targets, for CI, and for the reach of delivery and distribution. Dune’s package-management features allow Dune to orchestrate the build of a project’s entire dependency closure, including the OCaml compiler. This presents an opportunity for Dune to automatically cross-compile a project’s library dependencies without the need for explicit ports of library packages, and for Dune to install the appropriate OCaml cross-compiler.
**Target completion**: Q2 of 2027
**Tracked by:** [First-class cross-compilation via dune package management · Issue #14731](https://github.com/ocaml/dune/issues/14731)

### 5. First-class OxCaml Support **TODO**

**Why this matters:** OxCaml is an important space for innovation in the OCaml ecosystem. As OxCaml continues to develop, proving first-class support for the compiler variant will make it more accessible for people to explore its power, and reduce friction points that may otherwise contribute to ecosystem fragmentation.
**Target completion**: Q1 of 2027
**Tracked by:** [Support project initialliation scaffolding · Issue #12100](https://github.com/ocaml/dune/issues/12100)

### 6. First-class vendoring **TODO**

**Why this matters:** Vendoring gives developers the flexibility to reduce their dependency cone, make adjustments to dependencies, and rapidly develop changes that can be contributed back upstream. It is also a required part of workflows used by many established OCaml projects, from Mirage to Dune.
**Target completion**: Q2 of 2027
**Tracked by:** [Future of vendoring in Dune · Issue #3909](https://github.com/ocaml/dune/issues/3909)
