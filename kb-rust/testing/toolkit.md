# Rust Testing

**Language:** Rust · **Category:** Testing

Strategy: kb-core/patterns/testing-strategy. Here: Rust toolkit.

**What:** built-in `#[test]`, `proptest` property, `mockall` mocks, `rstest` params, `criterion` benches.

```rust
#[rstest] #[case(1,2,3)] #[case(0,0,0)]
fn adds(#[case] a: i32, #[case] b: i32, #[case] want: i32) {
    assert_eq!(add(a, b), want);
}
```

**Toolkit:** `cargo test`, `#[cfg(test)]` modules, `assert!`, integration in `tests/`, criterion. **Gotchas:** tests run parallel by default; `--test-threads=1` if shared.

**See also:** kb-core testing-strategy.
