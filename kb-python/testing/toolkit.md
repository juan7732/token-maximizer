# Python Testing

**Language:** Python · **Category:** Testing

Strategy: kb-core/patterns/testing-strategy. Here: Python toolkit.

**What:** pytest, `hypothesis` property, `factory_boy` fixtures, `respx`/`responses` HTTP, `testcontainers`.

```python
@pytest.mark.parametrize("a,b,want", [(1,2,3),(0,0,0)])
def test_add(a, b, want):
    assert add(a, b) == want
```

**Toolkit:** fixtures + conftest, `monkeypatch`, `pytest -n` parallel, markers. **Gotchas:** fixture scope leaks state; mock where used not defined; freeze time for clocks.

**See also:** kb-core testing-strategy.
