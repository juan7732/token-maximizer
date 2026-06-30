# IDE Integration

**Category:** Local Models

**Continue.dev** (VS Code / JetBrains): point at Ollama.
```json
{ "models": [{ "title": "local", "provider": "ollama", "model": "qwen2.5-coder:7b" }],
  "tabAutocompleteModel": { "provider": "ollama", "model": "qwen2.5-coder:1.5b" } }
```
Small fast model for autocomplete, larger for chat.

**Neovim:** codecompanion.nvim or gen.nvim → Ollama endpoint.

**Routing local vs cloud:**
| Task                                   | Where |
| -------------------------------------- | ----- |
| autocomplete, boilerplate, rename      | local |
| private/proprietary code               | local |
| hard reasoning, large context, agentic | cloud |
| offline                                | local |

**Shell aliases:**
```sh
alias ask='ollama run qwen2.5-coder:7b'
alias explain='f(){ ollama run qwen2.5-coder:7b "explain: $(cat $1)"; }; f'
```

**Gotchas:** autocomplete needs a small model or it lags; keep daemon warm; cloud for big agent loops.

**See also:** ollama-apple-silicon.md, advanced.md.
