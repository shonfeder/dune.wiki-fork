## Present at the Meeting:

- Arseniy Alekseyev (@aalekseyev)
- François Bobot (@bobot)
- Jérémie Dimino (@jeremiedimino)
- Emilio Jesús Gallego Arias (@ejgallego)
- Andrey Mokhov (@snowleopard)
- Nicolás Ojeda Bär (@nojb)
- Rudi Grinberg (@rgrinberg)
- Anil Madhavapeddy (@avsm)

## CI

The latest version of ppx_expect breaks dune's tests. We don't know the root of the issue yet, but we've downgraded ppx to keep CI passing. We've implemented a new CI policy that is now active:

_No PR's failing CI shall be merged._

Consequently, if CI breaks our first priority is to fix it.

There was renewed interested in [github actions](https://github.com/ocaml/dune/pull/3451) because of the alleged reliability improvements it offers. We might adopt it even at the cost of performance penalties.

## Instrumentation

nojb implemented the [instrumentation proposal](https://github.com/ocaml/dune/pull/3535) that generalizes the bisect support to support different instrumentation backends. There was a discussion of the various advantages and disadvantages of this approach, and avenues for future improvement:

- Many instrumentation systems (runtime variants, fdo) require additional targets. We don't support this feature at the moment.

- It was brought up that custom backends are too complicated for just ppx based instrumentation. We could just add conditional preprocessing. However, we'd lose flexibility with customizing backends in the future and would have to refer to the instrumentation program by the ppx library rather than the package (`landmarks` vs. `landmarks.ppx`).

## Jenga Migration

Jeremie remarked that janestreet needs a better dune build API to migrate from jenga. This API should be much better designed & stable. Eventually, it might even be offered to end users. Jeremie will create a ticket to discuss the API itself.

## Vendoring

We discussed how we manage packages when developing dune. It seems to be much more convenient to work with upstream versions of every package in development. While in release mode, the packages inside dune can be used.

Francois pointed out that it complicates keeping the versions of packages in sync.

The team would like to see how the duniverse tools develops before we commit to a solution here.