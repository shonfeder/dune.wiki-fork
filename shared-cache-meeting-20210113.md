# Present at the meeting:

* Andrey Mokhov (@snowleopard)
* Quentin Hocquet (@mefyl)

# Discussed

Jane Street is switching to internal development of the distributed cache for faster iteration and easier integration with internal systems: no need to deal with OPAM and Dune -- everything can be built with a single Jenga command, it's easy to reuse code for manipulating Jenga's shared cache for atomic shared cache read/write logic, metadata parsing, etc. 

Since the previous meeting, Andrey implemented an internal distributed cache prototype that inherits the design and implementation ideas of the Dune distributed cache but is rewritten from scratch. It uses a blocking strategy for downloading artifacts (i.e. we check the cloud cache before building a rule), which gives promising results: roughly ~2.5x speed-up when building a project from scratch using the remote cache. The internal network is fast enough for the roundtrip Jenga-daemon-server-daemon-Jenga to take less than 1ms, so the overhead due to blocking appears to be pretty small.

Once the internal development is complete, Jane Street plans to opensource the developed system and integrate it with Dune.

We are going to stop weekly distributed cache meetings.