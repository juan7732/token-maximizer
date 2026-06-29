# Python Error Handling

**Language:** Python · **Category:** Error Handling

Strategy: kb-core/patterns/error-handling. Here: Python mechanics.

**What:** base exception + subclasses, raise with context, catch narrow. `raise from` keeps cause, context managers clean up, `ExceptionGroup` for concurrent failures.

**Why:** clean happy path; hierarchy lets callers catch at the right grain. Cost: discipline against broad `except`.

```python
class AppError(Exception): ...
class NotFound(AppError): ...

def renew(uid: str) -> User:
    try: return store.load(uid)
    except KeyError as e: raise NotFound(uid) from e  # keep cause

with open(path) as f: process(f)                      # structural cleanup
try: renew("42")
except NotFound: return 404                            # narrow, map at boundary
```

**Gotchas:** never bare `except`; `from None` drops cause deliberately; don't use exceptions for normal flow; 3.11+ `except*` for `TaskGroup`.

**See also:** kb-core error-handling; kb-go/rust/typescript idioms.
