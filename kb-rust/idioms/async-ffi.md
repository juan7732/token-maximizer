# Rust Async & FFI

**Language:** Rust · **Category:** Idioms

**Async:** tokio runtime; `.await` yields; avoid blocking in async (use `spawn_blocking`). Cancellation = drop future. `Send` futures cross threads.

**FFI:** `extern "C"`, `#[no_mangle]`, `repr(C)`; unsafe at boundary, safe wrapper around. Free what you alloc. cbindgen for headers.

```rust
#[tokio::main] async fn main(){ tokio::spawn(work()).await.unwrap(); }
#[no_mangle] pub extern "C" fn add(a:i32,b:i32)->i32{a+b}
```

**Gotchas:** blocking starves executor; nested runtimes panic; FFI leaks; null derefs in C interop.

**See also:** concurrency.md, traits-generics.md.
