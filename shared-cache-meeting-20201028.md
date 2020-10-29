# Present at the meeting:

* Arseniy Alekseyev (@aalekseyev)
* Jérémie Dimino (@jeremiedimino)
* Quentin Hocquet (@mefyl)

# Discussed

* The cache daemon and distributed artifacts daemon now communicate with Async RPC, with good performances (3x on the dune repository dune itself). It looks like it might saturate the network as expected.
* Quentin will bench on JS universe too.
* Files are still transmitted with strings, sendfile is being integrated in Async RPC by Andrey, Pipe RPC are a solution too.
* Quentin will start looking at kerberos integration too.