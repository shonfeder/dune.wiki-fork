# Present

- Jeremie Dimino (@diml)
- Cameron Wong (@cwong-ocaml)
- Lubega Simon (@lubegasimon)
- Rudi Grinberg (@rgrinberg)
- Francois Bobot (@bobot)
- Nicolás Ojeda Bär (@nojb)
- Emilio (@ejgallego)

# Process

For this week's meeting, we followed the new format proposed by @diml last time, in which we discuss and assign recent issues.

# Extend `enabled-if` stanza (#3832)

Jeremie is already working on a similar feature. It is somewhat unclear what the semantics of `enabled-if` should be in the general case. The particular use case discussed was in determining whether a particular test should be run based on some configuration only known at build-time, so it seems like a worthwhile feature to pursue. François will take point on moving this forwards.

# Stat error on broken symlink during dune scanning (#3819)

This is a similar error to an earlier bug where special files (such as a unix socket) would cause issues, which has since been fixed.

Currently, dune ignores these special files, so it would be consistent to also ignore broken symlinks. However, this may be surprising behavior. It may instead make sense to treat symlinks as first-class, opaque objects to be manipulated and fail out when the symlink is dereferenced. Ideally, we would eagerly warn when processing a broken link and lazily error out.

In any case, broken symlinks feel like they should behave the same as otherwise-corrupted files. Dune should do the same thing when processing files with incorrect permission bits, which are currently ignored.

This also may be related to an error with filenames on Windows. Arseniy is investigating.

# debuggable targets, pretty printing (#3818)

It is currently difficult to cajole dune into handling certain types of debuggable targets. The issue reporter suggested that we mimick the behavior of ocamlbuild, which accepts `.itarget` and `.d.itarget` files. As the dune team is currently unfamiliar with how these works, the request is difficult to understand, so Rudi will be taking charge of driving this discussion.

# ocp-indent support (#3811)

Dune currently only supports ocamlformat with `dune build @fmt`. As many projects currently use ocp-indent, it would be nice to add first-class support.

It would be simple to patch in hardcoded support for ocp-indent by replacing the `ocamlformat` command internally. However, dune does not currently support much formatting configuration. The `fmt` action is a top-level one that does not have access to the project settings, so a more general change would be nontrivial.

Once dune is fully adopted within Jane Street, there will be a similar problem when running their internal `apply-style` formatter. Instead of attacking these individually, it would be better to tackle the more general problem of plug-and-play formatters.

We can take inspiration from inline-tests. Dune is not aware of `ppx-expect` the same way that it is currently aware of `ocamlformat`, but instead admits an API that can be programmed against. When talking about formatters, we would have an instance each for `ocamlformat`, `ocp-indent`, and `apply-style` against this API.

As we are exploring the more general solution, we are postponing this issue for now. However, if there is a large number of projects using ocp-indent that would benefit, we may implement something simpler as a stop-gap.

# dune-runtest failure when DUNE_BUILD_DIR is set (#3805)

This is most likely a more general bug related to sandboxing. This will need further investigation from the reproduction test given in the issue. Assigned at random to Cameron.

# Test stanza forgets deps aliases (#3801)

Jeremie will investigate.

# "No rule found" when migrating dune (#3800)

This is possibly already solved. Nicolás will investigate if not.

# opam file generation (#3792)

This is related to the one-package-per-dir feature, which will solve this. For now, you can add the package field to the cram stanza.

# Process

There was further discussion about whether this meeting was productive. General points raised:
- "random" issue assignment needs to be weighted according to the assignee's bandwidth for dune
- issues are currently addressed newest-to-oldest. to prevent older issues from being buried, once the issue-assigning process has been finalized, it will be applied retroactively to all outstanding issues. Ideally, this would leave a manageable number of issues to be handled every meeting
- `master` should be in a "good" state at all times. PRs, then, will be merged under the condition that they "improve" `master`, excepting features that span multiple branches (in which case there may be intermediate stages).