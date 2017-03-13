This page explains how to accomplish common things that are not built-in Jbuilder.

Saving the git revision in the executable
-----------------------------------------

The following jbuild file will generate a rule that writes the current package version and git revision in a `version.ml` file.

The idea is to use the OCaml syntax for jbuild files as described in the manual to call git and keep a `version.ml` file up-to-date with the current git revision. What it does is that it systematically calls `git log ...` to extract  the commit hash of the current head and generates a rule to produce the `version.ml` file that hard-code this commit hash.

After the first build, if jbuilder see that the rule has changed, it will automatically execute it again to produce an up-to-date `version.ml` file.

```ocaml
(* -*- tuareg -*- *)
#require "unix"
let git_version =
  if not (try Sys.is_directory ".git" with _ -> false)
  then ""
  else
    let ic = Unix.open_process_in "git log -n1 --pretty=format:%h" in
    let version = input_line ic in
    close_in ic;
    version

let version =
  let ic = open_in "VERSION" in
  let version = input_line ic in
  close_in ic;
  version

let () = Printf.ksprintf Jbuild_plugin.V1.send {|
(rule
 ((targets (version.ml))
  (deps ())
  (action (with-stdout-to ${@}
           (echo "let s = \"%s\"\nlet git = \"%s\"")))))
|} version git_version
```