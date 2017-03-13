This page explains how to accomplish common things that are not built-in Jbuilder.

Saving the git revision in the executable
-----------------------------------------
`cat jbuild`
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