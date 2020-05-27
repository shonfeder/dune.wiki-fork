# Present at the meeting:

Andrey Mokhov (@snowleopard)
Arseniy Alekseyev (@aalekseyev)
Jérémie Dimino (@jeremiedimino)
Quentin Hocquet (@mefyl)

# Summary

## Metadata files and no-replication

The design document for block store needs some clarification
regarding content-addressable artifacts vs non-content-addressable metadata files.
In particular, for metadata files:
- we should clarify that the block keys are not content hashes for metadata files.
- we should clarify what happens in the case of non-determinism.

The single-box-per-block also then becomes a desirable
correctness property for metadata files (so we can detect collisions more reliably) instead
of being just a design simplification.

We discussed the limitations of there being no replication and agreed that they are
not problematic.

## Reverse proxy

If we use a reverse proxy, this lets us decouple certain protocol considerations
from the main shared-cache daemon logic, in particular Kerberos authentication
can be done independently, so we won't need to link any code between 
Jane Street kerberos libraries and the shared cache daemon.

## Macbook hardlink issue

There's an issue with shared-cache that affects shared-cache on
macbooks when hardlinks are used.  This is being investigated.

## Incremental csexp parser

Csexp parser is currently not incremental
(doesn't support Lwt or Async). Quentin is going to add a version of
the parser thast is general over any monad.

# Discussed topics

- Discussed how metadata files are not content-addressable, so we
  should deal with non-determinism in a well-defined way.
  A question was raised what we should do on collision: should we keep
  new data, or old data, or do something else. 
  The conclusion we reached is that it's not very important because fixing
  a non-deterministic rule almost always involves changing the rule,
  not changing its outcome.
- Discussed if the data representation should be
  one-box-for-each-rule, or if there should be redundancy (more than
  one box stores the same rule)
- One challenge with redundant representation is the non-deterministic
  rules: you may end up with multiple boxes in the cluster disagreeing
  about which is the right answer to a given question, which could be
  hard to think about. With one-box-for-each-rule this is simpler
  because we can always detect a collision.
- Discussed the performance limitation imposed by that: Quentin claims
  that bandwidth or storage will be a limiting factor, and that scales
  well with the number of boxes, so scaling should not be a problem.
- Andrey points out that if everybody is waiting on one file, the box
  hosting that file can become a bottleneck, so scaling can become a
  problem at some point.
  Quentin claims this is unlikely to be an issue because one file will
  never contribute too much to the build, there will be other files to
  retrieve. Arseniy argues that this is only a problem if size of the
  artifact constitutes more than [b/n] of the build cost where [b] is
  the total cost of the build and [n] the number of cache boxes, which
  is expected to give us a very good headroom. Andrey seems to be
  mostly convinced.
- Arseniy mentioned that reliability might be another concern with
  one-box-for-each-rule approach, but Quentin argues that Dune's
  algorithm means that the worst possible case is that we build
  everything from scratch, which is not terrible. Also we'll generally
  still reuse the artifacts that are available even if some are not
  available, so overall we're not too worried.
- We discussed the reverse proxy design proposal.
  The idea is that it would let us decouple certain protocol aspects from the
  actual dune-cache binary.
  A reverse proxy (nginx was discussed) can implement TLS, presumably with Kerberos
  authentication support and potentially other things, like load balancing
  (it's not clear why we'd want load-balancing, though).
- Jeremie mentioned that if we do this, we won't even need to link the
  dune-cache binary with any Jane Street code, so we don't have to import
  dune-cache daemon to jane, so we don't have to migrate it to jenga
  build system.
- Quentin mentioned a confusing issue with macbooks, hardlinks
  and the daemon that is still being investigated.
- Jeremie mentioned: in Jane Street, we can already start writing the client-side
  integration of the protocol to talk to the daemon.
- We discussed csexp parser and how it's not currently incremental
  (doesn't support Lwt or Async). Quentin is going to add a version of
  the parser thast is general over any monad.
