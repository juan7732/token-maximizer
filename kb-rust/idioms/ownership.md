# Rust Ownership

**Language:** Rust · **Category:** Idioms

The mental model that unlocks the rest.

- **One owner.** Value dropped when owner leaves scope. Move transfers ownership.
- **Borrow:** `&` many readers OR `&mut` one writer, never both. Compile-checked.
- **Lifetimes:** how long a ref is valid; usually inferred, annotate when stored.
- **Clone to escape** a fight when cheap; don't over-`Arc`.

```rust
fn read(s: &str) {}          // borrow
fn take(s: String) {}        // move, caller loses s
let s = String::from("hi");
read(&s); take(s);           // s gone after
```

**Patterns:** newtype for invariants; `Cow` borrow-or-own; `Rc/Arc` shared, `Arc<Mutex>` shared-mut; iterators over loops.

**Gotchas:** fighting borrow checker = wrong shape, restructure not clone-spam; self-referential needs Pin; lifetimes in structs annotate.

**See also:** error-handling.md, concurrency.md.
