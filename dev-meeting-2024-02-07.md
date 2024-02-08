Present:
@electreaas
@emillon
@gridbugs
@leonidas-from-xiv

- using utf8 for dune files (@emillon)
  - refs: #9728, #9396
  - should we mandate utf8 encoding for dune files?
    - seems fine
  - if we do that, we can let the formatter output utf8
  - atoms are lexed more strictly and are already a subset of 7-bit ASCII, so utf8 extensions will mostly appear in quoted strings
  - things like library names are also validated so this is not about proposing utf8 in there
  - accepting BOM in output
    - BOM in utf8 is not really useful but some tools (especially on windows) emit it
    - it's not really possible to accept it (because it would break old dunes parsing new files)
    - but it's possible to detect it while parsing and emit a good error message

- progressive enhancement based on packages (@emillon)
  - some integrations can be "enhanced" optionally
  - 2 recent exemples:
    - if sherlodoc is installed, it is used by the docs rules
    - is ocaml-index is installed, it is used to build an index for merlin
  - it feels like a hack if uninstalling the package is the only option
  - ok for a first version, but needs to be moved to a config file or tool configuration (e.g. odoc)
  - one problem is that there's no good place to configure this

- pkg rules blockers (@leonidas-from-xiv)
  - actually not that many blockers
  - ocamlfind
  - ocaml compiler
    - main plan is to wait for relocatable patches
    - use system compiler in the meantime