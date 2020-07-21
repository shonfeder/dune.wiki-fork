# Present at the meeting:

* Andrey Mokhov (@snowleopard)
* Jérémie Dimino (@jeremiedimino)
* Quentin Hocquet (@mefyl)
* Arseniy Alekseyev (@aalekseyev)

# Work

## Quentin

* Working on the work duplication issue where the same file is re-downloaded multiple times if it's requested multiple times in parallel.
* Need further testing to make progress.

## Andrey

* We can connect the daemon to the storage node, jenga can connect to the daemon, uploads things to cache successfully.
* Downloading for some reason fails, investigating together with Quentin why that could be happening.

# Discussions

## Need good story for Kerberos.

On the server-side we decided we'll try to use nginx, but the client will need to talk Kerberos directly.

One way is to open-source Jane Street Kerberos stuff and use it in dune-cache

Another is to have an abstract authentication layer in dune-cache which we fill in in Jane Street by replacing a module or applying a functor.

Maybe it's possible to use an alternative Krb library, but that seems undesirable because of security concerns.

# Async vs Lwt

One challenge is that Krb library in JS is written with Async, not Lwt.

If we're to use it, we will probably need to rewrite at least the shared-cache client to use Async.

There's an async-lwt compatibility layer, but it's a big experimental project
that's never been used in a real-life app, so it's scary to use it.

We could also have a separate process that runs Async and communicates to Lwt, but that seems to make the client side overly complicated.