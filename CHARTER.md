# Portable SIMD Charter

Build and stabilize a portable SIMD API to the standard library under `core::simd` and `std::simd`.
Draw elements from the existing portable SIMD implementations out in the ecosystem:

- [`generic-simd`]
- [`packed_simd`]
- [`wide`]

## Goals

- Determine the shape of the portable SIMD API.
- Get an unstable `std::simd` and `core::simd` API in the standard library.
This may mean renaming `packed_simd` to `stdsimd` and working directly on it, or creating a new repository and pulling in chunks of code as needed.
- Produce a stabilization plan to allow portions of the API to be stabilized when they're ready, and coordinate with other unstable features.
- Respond to user feedback and review contributions to the API.
- Update [RFC 2948] based on the final API and stabilization plan.
- Stabilize!

## Constraints And Considerations

The initial implementation will be built on LLVM intrinsics directly rather than `core::arch`.
This is so we don't have to block the portable API on filling in a lot of missing intrinsics in `core::arch` on non-x86 platforms.

## Membership

**Shepherds:** @hsivonen, @KodrAus, @Lokathor
**Team Liason:** @KodrAus
**Members:** @BurntSushi, @calebzulawski, @hsivonen, @KodrAus, @Lokathor, @workingjubilee

[`packed_simd`]: https://github.com/rust-lang/packed_simd
[`wide`]: https://github.com/Lokathor/wide
[`generic-simd`]: https://github.com/calebzulawski/generic-simd
[RFC 2948]: https://github.com/rust-lang/rfcs/pull/2948
