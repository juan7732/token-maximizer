# Ollama on Apple Silicon

**Category:** Local Models

```sh
brew install ollama
ollama serve            # daemon on :11434
ollama run qwen2.5-coder:7b
```

**Model by unified RAM** (leave headroom for OS/apps):
| RAM   | Coder model | Quant       |
| ----- | ----------- | ----------- |
| 16GB  | 7B          | Q4_K_M      |
| 32GB  | 14B         | Q4_K_M / Q5 |
| 64GB  | 32B         | Q4_K_M      |
| 128GB | 70B         | Q4_K_M      |

**Quantization:** Q4_K_M = best size/quality default. Q5/Q6 if RAM spare. Q8 near-fp16. Lower = faster + dumber.

**Picks:** qwen2.5-coder (code), llama3.1 (general), nomic-embed-text (RAG embeddings).

**Modelfile** (custom system prompt + params):
```
FROM qwen2.5-coder:7b
PARAMETER temperature 0.2
PARAMETER num_ctx 8192
SYSTEM "Terse senior engineer. Code first, no filler."
```
`ollama create myc -f Modelfile`

**Gotchas:** context length eats RAM (num_ctx); first load slow; Metal only, no GPU sharing; quant below Q4 degrades fast.

**See also:** ide-integration.md.
