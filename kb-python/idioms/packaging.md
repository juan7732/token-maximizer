# Python Packaging

**Language:** Python · **Category:** Idioms

- `pyproject.toml` single source; `src/` layout.
- **uv** default (fast); poetry if needed; pin + lock always.
- venv per project; never global installs.
- Ruff lint+format, mypy/pyright types.

```
pyproject.toml
src/pkg/__init__.py
tests/
```

**Gotchas:** flat layout imports test-vs-installed mismatch; pin transitive; `__init__` markers.

**See also:** modern.md.
