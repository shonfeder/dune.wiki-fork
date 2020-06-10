# Present at the meeting:
Andrey Mokhov (@snowleopard)
Arseniy Alekseyev (@aalekseyev)
Jérémie Dimino (@jeremiedimino)
Quentin Hocquet (@mefyl)

# Discussions

Quentin has a working implementation of a protocol for hydra machine discovery, implemented using httpaf.

This is a variation of distributed eventual consensus protocol, but it's simple
enough that we don't need the usual strong guarantees associated with consensus
protocols.

The off-the-shelf protocols (e.g. Chord) are complicated, and we can get away with something simpler.

The protocol we have works roughly like this:
- every machine maintains a set of all other machines
- machines talk to each other and every time there's a change (a new machine
is added or removed), the change is propagated peer-to-peer

We discussed a problem of artifact assignment to machines and how adding a
machine may change the assignment.

We want the assignment to be relatively stable: we don't want the addition of a single machine
to disrupt the whole cache. For example:

- a naive uniform splitting of artifact space between the machines will lead
to relocation of ~half of all artifacts.
- modulo-n hashing to assign artifacts to machines is even worse:
adding a machine will almost-randomize all hashes and change most of them.

We may want to use a [consistent hashing](https://en.wikipedia.org/wiki/Consistent_hashing) scheme.

One proposal is to use a hashing scheme where the artifact is assigned to the node
whose hash is closest to the hash of the artifact on some metric.

It was mentioned that it's the client who decides where to store the artifacts,
so we need to make the client aware of the node configuration before they can store
or lookup artifacts.

## Todo

On the distributed cache side:

- Http interface to push/get blocks. The protocol isn't finalized yet, but it's very small so should be easy to change if necessary.
- Local trimming of artifacts on cache nodes

On the Jane Street side:

- Andrey finished code changes integrating jenga with dune cache format,
we're about to roll these out
- We're looking at ways to share code between the trimming daemon we use in Jane
Street and the open-source one.

The plan is to use hydra distributed shared cache in July.