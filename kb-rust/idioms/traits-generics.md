# Rust Traits & Generics

**Language:** Rust · **Category:** Idioms

- Generics monomorphize (fast, big binary); `dyn` = vtable (small, dynamic).
- Bound on impls not structs. Use `where` for readability.
- Newtype to impl foreign trait on foreign type (orphan rule).
- Prefer `impl Trait` args/returns over boxing.
- Marker traits (Send/Sync) guide concurrency.

```rust
fn parse<T: FromStr>(s: &str) -> Result<T, T::Err> { s.parse() }
fn make() -> impl Iterator<Item=i32> { (0..3) }
trait Repo { fn get(&self,id:u64)->Option<User>; }
fn run(r: &dyn Repo) {}
```

**Gotchas:** orphan rule blocks impls; trait objects need object-safe methods; over-generic = bloat.

**See also:** ownership.md.
