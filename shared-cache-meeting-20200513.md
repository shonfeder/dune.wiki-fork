Present at the meeting:

- Andrey Mokhov (@snowleopard)
- Arseniy Alekseyev (@aalekseyev)
- Jérémie Dimino (@jeremiedimino)
- Quentin Hocquet (@mefyl)

## Testing

Quentin started testing the distributed shared cache on RWO. With a
fill distributed cache and no local shared cache, he is observing a
45% speed up compared to a build from scratch with nothing in
cache. There seems to be things being rebuilt un-necessarily, peharps
because some rules depends on the universe in RWO.

Here are the actual numbers:
- from scratch: time: 2:49.73 real  17:19.98 user  4:06.64 system (758%)
- with a full distributed cache and an empty local cache : time: 1:35.60 real  7:25.16 user  2:02.86 system (594%)
- with a full local cache: time: 35.057 real  2:44.00 user  14.287 system (508%)

That's a 45% boost with the remote cache, and 80% with the local cache (from a clean tree each time).

For this test, Quentin has been using a distributed cache located on a
Docker container on the same physical machine. The communication
between the local cache daemon and the distributed one in the Docker
container was done via WebDAV.  Irmin is crashing in some cases and
Quentin is going to investigate.

## Protocol and metadata

We discussed a bit the communication protocol. Currently, the local
machine sees the distributed cache the same way as the local one,
i.e. as a file system. Except that instead of using the posix file
system API to access it, it uses the network protocol, currently
WebDAV.

While WebDAV is simple and works well, there are some limitations; in
particular it doesn't seem easy to store the executable bit. There are
several choices:
- we could make up our own protocol
- we could make store such metadata in our metadata file

Quentin is going to write up a document describing the various options
and their pros and cons.

## Security

The communication protocol needs to be secured. We have libraries
inside Jane Street that handle that and that we will need to use in
the shared cache, at least in Jane Street internal build of the Dune
cache daemon.

One issue is that our libraries are using Async while the cache daemon
is using Lwt because Irmin uses Lwt. Although that shouldn't be a
blocker and we should be able to work something out.

## Storing outputs in the shared cache

Jenga supports two kinds of build rules: classic build rules that
produce targets and rules that produce no target and for which we
cache the output. We use the second kind of `ocamldep` among other
things. The local shared cache used inside Jane Street supports
storing outputs, so the distributed shared cache will need to support
it as well.

Andrey wrote a design doc for the local shared cache inside Jane
Street and is going to publish it in the Dune repo, after a pass to
remove the Jane Street specific things.

## Next steps

Quentin is going to continue testing on RWO, debug the Irmin crashes
and follow up on the protocol choice question.

Inside Jane Street, we are going to continue with our plan to deploy
the new shared cache format that supports deduplication. We are also
going to start importing the code of the dune-shared-cache project and
its dependencies inside our internal repository so that we can build
it and start testing it.
