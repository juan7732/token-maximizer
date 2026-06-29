# Rust Performance

**Language:** Rust · **Category:** Performance

Already fast; don't fight it. Profile: `cargo flamegraph`, criterion.

- **Zero-cost:** iterators, generics, no GC. Trust them.
- **Avoid alloc:** `&str` over `String`, `SmallVec`/`ArrayVec`, slices over Vec.
- **Clone aware:** clone is explicit; reduce, don't reflex-clone.
- **SIMD/rayon** for data parallel; `--release` always for bench.

**Gotchas:** debug build is slow, bench release only; `Box<dyn>` adds vtable, generics monomorphize; bounds-check elide via iterators.

**See also:** ownership.md, concurrency.md.
