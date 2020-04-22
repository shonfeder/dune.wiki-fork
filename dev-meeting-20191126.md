OCaml version compat
--------------------

Emilio mentions tht dropping support for 4.05.0 might make some users
unhappy. These are users that install OCaml via the official debian
package and then install dune, coq and other packages via packages
provided by third-party sources. Debian stable only has OCaml 4.05.0.

Indeed, providing third party packages for various OCaml packages is
fine, but providing a third-party package for the compiler itself is a
bit more tedious and controversial.

Increasing the window of supported OCaml versions to 4 versions would
mean that 4.05.0 would be supported for a bit longer. However, it
wouldn't be that long as we expect to move to 4.09 soon, i.e. as soon
as merlin supports 4.09.

Additionally, it is expected that most people are now using opam and
installing their OCaml compiler through opam. So it's not clear how
many people will be affected by dropping support for 4.05.0.

As a result, we decided to keep with the current plan for now. If the
worst came to the worst and our new policy was making things
impossible for users, we could always change it. As a result, there is
no urgency. In any case, we should keep an eye out for feedback from
users about this.

We are also thinking of exploring alternatives such as distributing
pre-built binaries of Dune or (@dra27 you should stop reading here if
you don't have spare time in the near future! :see_no_evil:) providing
a tarball that would include the sources of both Dune and a recent
version of the compiler and would build just enough of the compiler to
be able to build Dune, possibly always in bytecode and using only the
bootstrap compiler for simplicity.

Such solutions would probably not be acceptable for the official
packages from Linux distributions, but for third-party distributors
this would probably be much less of a concern.


Default warnings
----------------

We discussed the fact that dune enables a different set of warnings by
default in development mode, following a comment from Xavier Leroy. We
decided to keep the status quo, although Jeremie got the comment
slightly wrong; the complaint was about the fact that these are
treated as error rather than the set of enabled warnings.

Coq
---

Since we restarted the talks about plugins, Emilio is wondering about
relying on this for Coq, rather than have all the logic backed into
Dune.

We didn't talk much about this as this was the end of the meeting, but
making this possible would probably require adding a few more general
features in Dune, given that for now we are only talking about plugins
that can generate plain stanzas. i.e. they won't have access to Dune's
internal APIs.

Jenga -> Dune migration at Jane Street
--------------------------------------

JS is making progress with building its mono-repo with Dune. The
initial builds where failing after consuming all the memory
(>30GB). With the help of spacetime, it was easy to identify things
that could be shared and bring back the memory usage under control
(<5GB).
