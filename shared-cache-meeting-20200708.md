# Present at the meeting:

* Andrey Mokhov (@snowleopard)
* Jérémie Dimino (@jeremiedimino)
* Quentin Hocquet (@mefyl)
* Arseniy Alekseyev (@aalekseyev)

# Work

## Quentin

* Switch to the Csexp library.
* Add new "promoted" message.
* Support Unix sockets.
* Add trimming.
* Parse configuration file on the daemon side.

## Andrey

* Implemented the Jenga side of the cache daemon protocol.

# Discussion

## Naming

Talked about unifying names. Andrey is proposing Upload/Download. Quentin argues that Upload is overly specific because the implementation can choose to not upload or take additional actions, and that "promoted" or "inserted" makes less assumptions.

## Packaging

Quentin and Jeremie discussed the packaging of the daemon and dune in opam.
Daemon depends on libraries in dune package, which are intended to be internal,
providing no stable interface, which makes it necessary to pin daemon
to a specific version of dune, which is unfortunate.

Pinning dune manually will solve the problem until the code lands in the next release.

## Actual testing

We can already start testing, provided we either do not send the daemon `value` fields, or we change it to at least ignore them for now. Quentin will give instruction on how to build the daemon so we can start testing early next week.
