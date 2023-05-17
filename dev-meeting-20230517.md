Present: @alizter @emillon @leonidas-from-xiv

3.8 milestone:
- #7748 (serapi regression) - fix from Ali, Etienne to test fix
- #7747 (coq-stdlib regression): `coqc -config` shouldn't be called, Ali to have a look
- RPC threads: waiting for feedback from @nojb
- menhir issue:
  - we leave off 3.8 as it's a new feature and we don't have a good design yet
  - parsing flags is brittle and difficult (`(env)` in workspace doesn't have access to project metadata, etc)
  - `(explain)` field is nicer to work with, but it's also something we don't want to leave for releases
  - `dune ocaml menhir conflicts` command is fairly ad hoc but corresponds more to the one-off nature of this
  - maybe a `(strict)` mode that passes `--strict` (or somehow ensures no conflicts are left) and displays conflicts as they appear has better semantics
  - we'd need to have a menhir user or developer to discuss
  - performance implications of `--explain`?
- due date on milestone moved to t+2 weeks based on current state + resources + short weeks

debug features:
- #7743 merged too quickly - corresponding features are not in.
- #7752 opened to track this
- the rest is not part of 3.8

docs:
- importance of API docs that look good on ocaml.org
- new TOC: some kind of sorting would be good, but we can also use the main div on the right to point to important pages