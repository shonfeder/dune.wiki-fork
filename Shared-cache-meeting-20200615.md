## Present at the meeting:

Andrey Mokhov (@snowleopard)
Jérémie Dimino (@jeremiedimino)
Quentin Hocquet (@mefyl)

## Discussions

We settled on the following design:

There is a single flat configuration file describing the mapping from
set of hashes to machines. This file will live on the NFS and will be
read and monitored by all builder and storage nodes. Each node should
read the file on startup and poll it every minute so that manual
changes are picked up in a timely manner.

Nothing special will happen when a machine goes down accidentally. In
such case, it will be up to a system administrator to bring the
machine back up and running or decide to redistribute the hashes at a
more quiet time of the day. Until the machine is brought back up, we
will get a bit more cash misses. And since the distribution of files
is uniform, we expect that builds will get uniformly slower until the
machine is brought back up.

This approach seems better than automatically rebalancing the network
as the latter is complex and as the potential to be distruptive to the
network both when the machine goes down and when it is added again. We
discussed the idea of having automatic rebalancing after a long
timeout such as one day, though that doesn't seem necessary in the
case of Jane Street so we decided to keep things simple.

When a machine is manually added or removed, an administrator will
have to update the configuration file. This seems fine as such
operations are likely to be scheduled at quiet times, such as during
the weekend.

At this point, we don't plan to have a custom monitoring system to
detect when a storage node goes down. Instead, we will rely on our
existing infrastructure for that. Builders should performs network
read and write operations with a timeout. It is important to have a
timeout for both to avoid accumulating read/write operations when a
machine goes down.
