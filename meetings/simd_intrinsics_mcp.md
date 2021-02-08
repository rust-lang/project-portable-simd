# SIMD Intrinsics MCP

###### tags: `Portable SIMD`

## Summary

- Set an expectation that `simd_*` intrinsics from `extern "platform-intrinsic"` will form the basis of backend-agnostic intrinsics within the standard library.
- Fill in any missing gaps in existing `simd_*` intrinsics so that `core::simd` can avoid any direct LLVM dependency.
- Provide reference implementations for all `simd_*` intrinsics in miri that define their expected behavior. These reference implementations should be directly usable by `rustc` backends without having to manually translate them.

Implementation of this MCP will be coordinated between the Portable SIMD project and Compiler team for Cranelift and LLVM implementations.

## Motivations

The user-facing goal of the `core::simd` API is to let users take advantage of SIMD intrinsics in a way that's automatically portable across hardware. Realizing that goal requires special backend support so that `core::simd` can utilize existing intrinsics rather than having to target ISAs specifically. Since `rustc` has multiple compiler backends (LLVM and Cranelift) the `core::simd` implementation needs to also be portable across compiler backends.

We currently have a best-effort approach to backend-agnostic SIMD intrinsics through the `extern "platform-intrinsic"` mechanism. There are a number of existing functions like `simd_add` and `simd_mul` that are used throughout `core::arch`, but also many ISA idiosyncrasies that aren't reasonable to create `simd_*` intrinsics for so it also uses a mix of LLVM intrinsics and inline assembly. The story for `core::simd` is a bit different. It's goal is specifically to expose standard lowest-common-denominator functionality that's consistent across all supported ISAs. That makes `core::simd` a great fit for `simd_*` intrinsics, and `core::simd` would like to use them exclusively over LLVM intrinsics.

We can think of `core::simd` almost like a thin user-friendly wrapper over `simd_*` intrinsics. Since it's going to rely on these intrinsics and make promises about their behavior across platforms we should write reference implementations of them all in plain Rust. These reference implementations will both document the conformant behavior of these intrinsics and give compiler backends a way to support `core::simd` without necessarily having to implement intrinsics for all platforms.

## Implementation notes

We want to write reference implementations in plain Rust rather than in MIR, but also want backends to be able to use them to emit code. Is this something that's possible in the compiler? Is `compiler-builtins` the right place for them?

Are there special considerations for miri and const evaluation we should keep in mind? Ideally we should be able to make `core::simd` _really_ portable to anywhere you can run Rust code, including in const contexts.

## Who defines intrinsics?

The Portable SIMD group will be responsible for determining the behavior of the `simd_*` intrinsics it needs. We should design them in such a way that new intrinsics can be added without _requiring_ any changes any code in any backends to support them. Once the behavior of an intrinsic has been specified it becomes part of its stability story, since the reason they exist is to be consistent.

## What do we need from `T-compiler`?

We'll need a lot of help figuring out where to put reference implementations and how to make them available to the various backends.
