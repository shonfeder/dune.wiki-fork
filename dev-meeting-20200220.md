## Fiberisation

Rudi is continuing is work on spreading the Fiber monad through the
code. At the moment, he is refactoring the variable expansion
mechanism to use an applicative. We hope that this will allow to
squash the various passes, hide the boilerplate and make the code
easier to work with overall.

## Bundling

We decided to suspend that work for now because:

- everybody has different expectation and it is not clear what the best workflow is
- all the semantic can be implemented by tweaking the opam build instructions

So for now we are just going to wait and see how things develop. Once
we have more experience and we know better what work and what doesn't,
we will formalise it in Dune if necessary.

## Job server

Arseniy is currently looking at implementing the make job server
protocol which allows to share parallelism between various
processes. The original motivation for this work come from Jane
Street, so that we can squash several test rules together and reduce
the number of intermediate artifacts we keep on disk while still
preserving parallel execution. However, this feature will be generally
useful.

## Purifying Dune

Andrey is continuing his work on removing all the ad-hoc hash tables
and other global mutable data structure so that we can eventually
switch everything to the memorisation framework and safely share
results between runs in polling mode. At the same time, he is learning
various parts of Dune and proposing cleanups and improvements.

## Mdx

Nathan is almost done adding a mdx stanza to Dune. He recently did a
new release of mdx with everything that's needed for dune
support. It's now just a matter of finishing the review and merging
support for mdx in Dune. Initially, the feature will be guarded by
`(using mdx 0.1)` so that we have so margin to settle the final
details.

## Plugins

Nicolás proposed a simple mechanism for allowing to produce plugins
(.cma or .cmxs) from an `executable` stanza. By default, only the code
of the executbale itself will end up in the plugin, however it is
possible for the user to specify additional libraries to embed in the
plugin. This proposal only cover building the plugin, packacing and
distribution are left to the application developers. This proposal is
relatively simple and doesn't conflict with Francois' proposal, which
provides a full key-in-hand plugin mechanism.

So that users can install their plugin without having to think about
whether native dynlink is supported and which file they should
install, we need to introduce a new variable to denote the preferred
plugin extension. For instance `%{ext_plugin}` which would be `.cmxs`
if native dynlink is supported and `'cma` otherwise.

## Upgrading from Dune 1 to Dune 2

Ulysse is working on automated upgrading from Dune 1 to Dune 2,
reusing the infrastructure to upgrade from Jbuilder to Dune. For
instance, the upgrader will convert `alias` stanzas to `rule`
stanzas. It is not always possible to fully upgrade everything
automatically, however the goal is to do everything that can be done
automatically. And for the rest, we expect that messages emitted by
Dune, either during the upgrade process or error messages afterwards
will be enough for the user to make progress.

## Templating

Shon is planning to introduce a templating mechanism to `dune
init`. The goal is to allow users to easily share common project
patterns, such as a "web project" or "command line tool
project". `dune init` would setup some structure and let the user fill
the blanks.

## Cram test

Jeremie has been working on generalising the cram test framework so
that it can be used by users of Dune. This framework allows to write
integration tests and especially regression ones very easily given
that the syntax is very close to the shell, making it simple to learn.

The initial plan was to add a new `cram` stanza to support cram tests,
however we are contemplating the idea of adding various mechanism to
dune to reduce the boilerplate as much as possible. If defining a cram
test becomes as simple as:

    (rule (run dune-cram run.t))

Then there is no need for something more to learn for the user and
more importantly there is nothing special about cram. I.e., users can
define their own tool and use it as conveniently.

For the above to work, we discussed two ideas.

### Reporting corrected files to Dune

Right now, if an application produce or might produce a corrected file
we need to write:

    (action
     (progn
      (run ...)
      (diff? run.t run.t.corrected)))

We are thinking of adding a mechanism so that the application can
instead directly report to dune that it produced a corrected. This
way, the user only needs to write the `(run ...)` part and the rest
would be automatic. This mechanism in fact pretty much exist with
dynamic actions so it's just a matter of putting things in form.

### Letting Dune serve tools configuration

We realised that it is not needed for cram tests, however it might
still be useful for other applications. The idea is to write the
configuration of third-party tools directly in dune files and let dune
serve it to the tool. Dune becoming some kind of configuration manager
for build tools.

The end goal is to avoid the proliferation of configuration files to
learn and maintain, and reuse Dune well defined scoping rules. This
questions is still quite open and will need more thoughts.
