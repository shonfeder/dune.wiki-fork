# Present at the meeting:

* Andrey Mokhov (@snowleopard)
* Jérémie Dimino (@jeremiedimino)
* Quentin Hocquet (@mefyl)

# Discussed

* Performance where improved with HTTP keep-alive, but it's probably not as fast as could be.
* An experiment was made with H2, but Quentin couldn't get it to work correctly.
* We will switch to an async-rpc implementation to ensure we quickly get results. This will also ease krb integration.
* On the JS side, we could try again with Jenga just to make sure the system is functional and there is no race condition somewhere else.
