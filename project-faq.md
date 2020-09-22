
# Portable SIMD Project Frequently Asked Questions

## Will `stdsimd` use runtime feature detection?

No, that is not planned for direct inclusion in `stdsimd`.
Using dynamic feature detection has a small cost each time you check and then branch to one code path or the other.
Most operations within `stdsimd` are simply too small to justify that cost being a part of every function or method call.
Accordingly, the `stdsimd` crate only uses compiletime feature detection.
We suggest that you try using the [multiversion](https://docs.rs/multiversion/) crate in combination with `stdsimd` if you want to benefit from runtime feature detection.
Because the functions and methods in `stdsimd` are marked `#[inline]`, the actual code generation is performed in the *using* crate.
In other worse, `stdsimd` should combine with `multiversion` just as well as any other Rust code does, and correctly benefit from function multiversioning.
If you do find specific examples to the contrary, please report them to our issue tracker.
