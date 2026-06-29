# Rust Concurrency

**Language:** Rust · **Category:** Concurrency

Strategy: kb-core/patterns/concurrency. Here: Rust mechanics.

**What:** `tokio` async for I/O, `rayon` for CPU. `Send`/`Sync` enforce safety at compile time. `Arc<Mutex>` shared, channels preferred.

```rust
let sem = Arc::new(Semaphore::new(8));
let mut set = JoinSet::new();
for t in tasks {
    let p = sem.clone().acquire_owned().await?;
    set.spawn(async move { let _p = p; do_work(t).await });
}
while let Some(r) = set.join_next().await { r??; }
```

**Gotchas:** don't hold `Mutex` across `.await` (use tokio Mutex); blocking in async starves runtime (`spawn_blocking`); rayon CPU not I/O; cancel-drop a future mid-await.

**See also:** kb-core concurrency; kb-go/typescript/python.
