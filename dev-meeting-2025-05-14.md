## Agenda

- Long term plan of @rgrinberg and @alizter regarding Dune Blake3.
- Release 3.19.0?

Attendees: @rgrinberg @alizter @panglesd @shym @ejgallego @rikusilvola @maiste "Jason Ho" "Chukwuma Akunyili"

## Notes

### @rgrinberg and @alizter long plan term regarding Dune and Blake3 (@maiste)

- The goal is to vendor the Blake3 implementation in Dune.
- It is a small library so it shouldn't impact the size of the binary.
- We use `md5` from the standard library and Sha1 (and soon Blake3) are vendored.
- Benchmarks show an increase in performance using Blake3. It will be more noticeable on big codebase.
- The Blake3 algorithm scales better on new architectures (e.g. ARM) thanks to optimizations. Hence the performance boost.
- We may vendor `md5` to not rely on the version from the standard library.

### Release Dune 3.19.0 (@maiste)

- Blake3 requires the new stanza for foreign archive?
- It needs to be able to use assembly to be able to use the Blake3 algorithm.
- Question regarding the formatting feature introduce by @nojb.
- It is breaking Odoc and the PR was not merged but @panglesd is going to have a look at it.
- @rgrinberg increased the `lang dune` version to `3.20` so it won't be a problem to do the release.
- Future goal is to make the formatting in `dune` files depending on the `lang dune` version to avoid breaking changes.

### ODoc3 PR on Dune  (@panglesd)
- Install `ODoc3` using the sub dirs stanza
- Question from @rgrinberg regarding whether should keep or not the old rules.
- Right now, it doesn't support the assets and pages that are in sub folders but it displays a message to express some files are ignored.
- @jonludlam will have a look to add the new rules.
- Currently, the PR still uses the old rules and does not support ODoc3 new rules. We will be able to remove the old rules and display an error if the version of ODoc is too low when the ODoc 3 rules would be merged.


