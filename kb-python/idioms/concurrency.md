# Python Concurrency

**Language:** Python · **Category:** Concurrency

Strategy: kb-core/patterns/concurrency. Here: Python mechanics.

**What:** GIL = no thread parallelism for CPU. I/O → `asyncio`/threads; CPU → `multiprocessing`. 3.11+ `TaskGroup` for structured.

```python
async with asyncio.TaskGroup() as tg:        # cancels siblings on error
    for t in tasks: tg.create_task(do(t))

with ThreadPoolExecutor(8) as ex: ex.map(io_work, items)   # I/O
with ProcessPoolExecutor() as ex: ex.map(cpu_work, items)  # CPU
```

**Gotchas:** GIL blocks CPU threads; blocking call in async stalls loop; processes copy + pickle; `asyncio.Semaphore` to cap; cancel via `except CancelledError`.

**See also:** kb-core concurrency; kb-go/rust/typescript.
