# Python CLI Apps

**Language:** Python · **Category:** Idioms

- Typer for CLIs (typed, autocomplete); argparse if zero-dep.
- `[project.scripts]` entrypoint, not raw script.
- Config: env > file > flags; validate with Pydantic settings.
- Rich for output; exit codes meaningful.

```python
import typer
app = typer.Typer()
@app.command()
def build(src: str, force: bool = False): ...
```

**Gotchas:** mutable defaults; module side effects on import; pin `python_requires`.

**See also:** packaging.md, ecosystem/map.md.
