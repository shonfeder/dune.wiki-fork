# Present at the meeting:
- Arseniy Alekseyev (@aalekseyev)
- Andrey Mokhov (@snowleopard)
- Jérémie Dimino (@jeremiedimino)
- Quentin Hocquet (@mefyl)

# Discussion

- The distributed cache HTTP protocol implementation is working, client can connect to
it and send queries
- Currently one backend server, need to design a configuration file format to connect
to multiple servers
- Still need to figure out how each server finds itself in the configuration file.
Agreed that the hostname should do fine for self-identification.
- As for specification of hash assignment: we can have just a list of machines and
hash ranges assigned to each. Using a prefix of a hash could be a shorthand syntax
for specifying a range.
- Not much else is remaining to do on the Quentin side.
- Andrey was testing and cleaning up the new cache format in jenga
- We discussed access permission management for the future versions.
For now the idea is to have the reverse proxy do authentication and forward the username
to the cache daemon, but it's the cache daemon itself who will be responsible for
authorization by using the "repo" field in the metadata and knowing which users are allowed
to access which repos.
This has an undesirable consequence of the daemon having to understand metadata, but
that should be fine. In any case, none of this is going to be there in the first version.
(although we already have commit and repo names in the metadata, even if they are unused)

# Next steps

- On the Quentin's side, the next step is to implement the config file and the ability to
connect to multiple servers (~1 week ETA)
- On the Jane Street side, We need to write a client-side implementation of the protocol
between jenga and the cache daemon and then we can test inside Jane Street (~2 weeks ETA)
