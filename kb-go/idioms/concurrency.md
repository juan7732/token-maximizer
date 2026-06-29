# Go Concurrency

**Language:** Go · **Category:** Concurrency

Strategy: kb-core/patterns/concurrency. Here: Go mechanics.

**What:** goroutines + channels; share by communicating. `errgroup` for grouped tasks, context for cancel.

```go
g, ctx := errgroup.WithContext(ctx)
sem := make(chan struct{}, 8)        // worker cap
for _, t := range tasks {
    t := t; sem <- struct{}{}
    g.Go(func() error { defer func(){ <-sem }(); return do(ctx, t) })
}
err := g.Wait()                       // first error cancels ctx
```

**Gotchas:** leaks if no exit/cancel; loop-var capture (pre-1.22); close once, sender closes; `-race` in CI; never share without mutex/channel.

**See also:** kb-core concurrency; kb-rust/typescript/python.
