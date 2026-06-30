# Advanced Local Inference

**Category:** Local Models

**llama.cpp** - raw control, GGUF models, Metal accel. `llama-server` exposes OpenAI-compatible API. Tune `-ngl` (gpu layers), `-c` (ctx), `-t` (threads). More knobs than Ollama (which wraps it).

**RAG over this KB:**
1. Chunk markdown (by heading), embed with nomic-embed-text.
2. Store in sqlite-vec / LanceDB / Chroma.
3. Query → top-k chunks → stuff into context → answer.
- Token-efficient: retrieve only relevant kb-* docs instead of dumping all.

**Tool use:** models with function-calling (qwen2.5, llama3.1) → JSON tool schema → execute → feed result back. Keep tools few and typed.

**Fine-tune (MLX, Apple Silicon):** LoRA/QLoRA on small models for style/format. `mlx_lm.lora`. Use for narrow tasks; RAG usually beats fine-tune for knowledge.

**Gotchas:** RAG retrieval quality > model size; embedding + chunking matter most; fine-tune is last resort; context window caps stuffed chunks.

**See also:** ide-integration.md, kb-comparisons/.
