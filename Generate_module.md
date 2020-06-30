# Generate module for build info #

Date: 30/06/2020

Presents:

- François Bobot (@bobot)
- Jérémie Dimino (@jeremiedimino)
- Ulysse Gérard (@voodoos)

In PR #3104 (relocation) and PR #3523 (custom build info) both needs data to be passed from the build time to the execution time. With a generated module as API (PR #3104 solution) compared to a global table (solution used in PR #3523), the name of the data is statically verified by the compiler instead of having the lookup done dynamically. So the generated module API is prefered for the two cases.

Still generating a module directly with the data is avoided because it disallows to share the cache in useful cases (e.g different working directory).

The time of the computation of this data and where it needs to be available is different for the two cases

* For relocation:
 * Data known since generation (local/distant install path)
 * Correct data needed in cmxs.

* custom build info:
 * Data computed by a user action when the executable is linked
 * It seems contrary to having this data in a dynamic library
 * Data different for each executable and copy rules should work

A consequence for custom build info is to forbid the generation of cmxs in that case


## Conclusion ##

Have two separates stanzas:

* PR relocation:

```
(generate_special_module
 (module sites1)
 (plugins (c plugins))
 workspace_path
)
```

Relocation:
1. ML and MLI generation with placeholders
3. PROMOTE replace the placeholder


* PR custom_build_info:

```
(generate_custom_build_info
  (module ...)
  (max_size 65)
  (link_time_action (echo "Some custom information accessible with `Build_info.V2.custom ()`"))
)
```
Option1:
1. MLI generation no placeholder
2. LINK, ML generation placeholder (identified by binary_name), execution of the action -> register the information
3. PROMOTE replace with the information

## Futur extension possible ##

 * `generate_special_module`:
   * Add custom user action, executed during generation, but using placeholder for the cache.
   * Add build info (e.g version of library) available in .cmxs
