Present: Marek, Robin, Ali, Chukwuma, Shon, Sudha, Ambre

- Release of 3.24 (Shon)
  - Only https://github.com/ocaml/dune/issues/14724, and it's a test issue
    - Patch the released version of mdx
    - 
- Codifying review practices (Ali)
  - To help address repetition by reviewers
  - How can we reduce unnecessary repetitions from reviewers?
    - If we have a document detailing common pitfalls, we can get agents to
    - Ali wants to start writing a set of instructions to guide LLMs in doing code review
  - Having this documented could also help avoid conflicting reviews from people 
  - Low hanging fruit we can semi-automate:
    - Changelog entries (check lists and CI, checks)
  - Shon: isn't the main thing being proposed here some domain-specific best practices?
     - Should that not be included in hacking.rst?
     - 
- Automating more in dune repo (Ali)
  - E.g.,
    - Have agents generating reproduction cases
    - Automating generation of parser errors
    - Identifying duplicates
    - People should be in the loop, but agents can help with classification
    - Structured review by an agent could be helpful, and save review time
    - Applying labels
  - Robin:
    - sounds like a good idea, I've found LLM agents give useful review feedback
  - Ali:
    - important we limit this to safe cases, and not making decisions for us
    - not writing followups to issue authors, shouldn't impact human communications
    - thinks feedback should not be in the repo itself but somewhere that users somehow wont see it
      - shon: not clear how this would work
  - Shon:
    - strict classification work is unproblematic, IMO
    - but any generation, e.g., of reproduction cases, will impact human communication
      - doesn't mean it can't be useful, but if we are outsourcing the understanding of reports and production of repro, we are likely to lose some of the touch points for forming deep sympathies and understanding of user needs and uses cases
      - if there are lots of bugs that are very straight forward, we should be working on fixing the core quality and correctness
  - Sudha: will move backlog to reviewing reproduction cases
  - Was mentioned that there are lots of edge cases with parser
    - Shon: Proposed that in this case we should consider rigging up some PBT tests to exercise the language and surface all the edge cases
- Dependency update model (Sudha)
  - https://github.com/ocaml/dune/issues/13011
  - Sudha, enumerating proposals discussed in the issue
  - Robin:
    - Agreed that the automated updates between builds is not good
    - You don't want your build getting fixed between two different invocations
    - Also this is the model used elsewhere so it is easily understood
  - Shon shared context on some things that put us in a slightly different position than other ecosystems
  - Ali's: proposal
    - get rid of the "lockless" mode: lock directory must be in worktree
    - also adopt the proposed semantics for the lock directory whereby it doesn't try to update unless there are changes to deps
- Refined dependency analysis (Shon)
  - Why PRs into separate branch rather than into main?
    - This is just for 2 which need to land together to avoid breaking tests
---

* Autolocking Dependency update model (Sudha)
  * Problem ([#13011](https://github.com/ocaml/dune/issues/13011)): When Auto-locking is enabled, Dune commands like dune build, dune runtest , etc. silently re-lock dependencies against upstream opam-repository. This could lead to consequences like compiler updates when one is just running tests. It makes local builds depend on the mutable opam-repo universe. This has been reported by some users like @samoht.
  * Proposed solutions: After several discussions, there are several proposed solutions.
  1. Change autolocking update model: Update the dependency solution only when user requests for it, or dependencies change.
  2. Introduce a new auto-locking mode: Preserve the current auto-locking mode as lockless mode, and introduce an auto-locking mode where the first build generates lockfiles in the source tree.
  3. Configurable lazy or eager updates: Make it possible for users to choose whether they want eager updates (every time dune build runs) or lazy (when they request, or dependencies change)
  4. Update only in specific intervals: Don't update from opam-repo on every build, do it in an interval like one day or two days, etc.  
  5. Updates running in the background: Without interrupting user commands, updates can happen in the background.