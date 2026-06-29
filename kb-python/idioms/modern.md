# Modern Python

**Language:** Python · **Category:** Idioms

Idiomatic 3.11+.

- **Dataclass / Pydantic:** dataclass for plain, Pydantic at boundaries.
- **Match:** structural `match/case` on shape.
- **Protocols:** structural typing, duck without ABC.
- **Type hints everywhere;** check with mypy/pyright.
- **`__slots__`** for many-instance memory.
- **Comprehensions, generators, pathlib, f-strings.**

```python
@dataclass(slots=True)
class Point: x: int; y: int

match cmd:
    case Move(x, y): ...
    case Quit(): ...
```

**Gotchas:** mutable default args; late closure binding; relative imports; pin deps (uv/poetry); GIL for CPU (kb-core concurrency).

**See also:** validation.md, concurrency.md, error-handling.md.
