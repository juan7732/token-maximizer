# 30 Before 30: Project Execution & Strategy Guide

> **Purpose:** Turn a bucket list into a structured execution plan with dependency mapping,
> skill overlap analysis, and concrete next actions — maximizing LLM token spend along the way.
>
> **Key Insight:** These 30 items aren't independent. They cluster into reinforcing groups
> where completing one accelerates others. This plan exploits those overlaps ruthlessly.

---

## Table of Contents

1. [Project Cluster Map](#1-project-cluster-map)
2. [Cluster Deep-Dives & Execution Plans](#2-cluster-deep-dives)
3. [The Startup Concept: AI-Native Knowledge Graph](#3-startup-concept)
4. [The Handwriting Recognition Task Pad](#4-handwriting-task-pad)
5. [Token Spend Strategy](#5-token-spend-strategy)
6. [Master Timeline](#6-master-timeline)
7. [Prompt Library](#7-prompt-library)
8. [Appendices](#8-appendices)

---

## 1. Project Cluster Map

Your 30 items naturally group into 7 clusters. Items within a cluster share skills,
tools, or outputs that feed into each other.

### Cluster A: Software & AI Core
> *These build your engineering portfolio, startup foundation, and career growth.*

| # | Project | Key Skills | Feeds Into |
|---|---------|-----------|------------|
| 1 | Build a deep learning model | Python, PyTorch/MLX, data pipelines | #20, #4-pad, Startup |
| 2 | Build 3 tools (library, finance, time/mgmt) | Go/TS/Rust, full-stack | #8, #12, Startup |
| 5 | Make a video game | Rust/TS, game loops, graphics | #8, #12 |
| 8 | Open-source something useful | Any language, packaging, docs | #6 (portfolio) |
| 12 | Make an iPhone app (30 Under 30) | Swift/TS (React Native), mobile dev | #2 |
| 20 | Compete in a Kaggle competition | Python, ML, data science | #1 |
| 🆕 | AI-native knowledge graph (startup) | All languages, AI/ML, product design | #1, #2, #8 |
| 🆕 | Handwriting recognition task pad | ML, CV, hardware, embedded | #1, #26 |

### Cluster B: Hardware & Physical Computing
> *Tangible builds that blend electronics, 3D printing, and software.*

| # | Project | Key Skills | Feeds Into |
|---|---------|-----------|------------|
| 11 | Esfand's streamer wheel | Electronics, mechanical, embedded | #28 |
| 22 | 3D print a custom model | CAD (Fusion 360/OpenSCAD), slicing | #26, #28, #29 |
| 26 | E-ink art display (desk PoC) | ESP32/RPi, e-ink driver, Python/Rust | #2 (time/mgmt tool) |
| 28 | JoJo Arrow PCB art piece | KiCad, PCB design, soldering | #11 |
| 🆕 | Handwriting task pad (hardware side) | E-ink/tablet, stylus input, MCU | #26 |

### Cluster C: Creative & Visual
> *Photography, design, illustration, and creative coding.*

| # | Project | Key Skills | Feeds Into |
|---|---------|-----------|------------|
| 3 | 10 amazing photos | Camera technique, editing, printing | #27 |
| 25 | Create a font | Type design (FontForge/Glyphs), vector | #27 |
| 27 | Write & illustrate a bestiary | Writing, illustration, layout | #16 |
| 30 | Digital twin of house | 3D scanning, gaussian splatting, NeRF | #1 (applied ML) |

### Cluster D: Narrative & Writing
> *Writing, worldbuilding, and creative expression.*

| # | Project | Key Skills | Feeds Into |
|---|---------|-----------|------------|
| 15 | Make a song | DAW (Logic/Ableton), music theory | #5 (game audio) |
| 16 | Write short fiction | Creative writing, editing | #27 |
| 27 | Bestiary / lore document | Worldbuilding, illustration | #5 (game lore) |

### Cluster E: Physical & Craft
> *Hands-on making, fitness, and outdoor activities.*

| # | Project | Key Skills | Feeds Into |
|---|---------|-----------|------------|
| 7 | Get to 220 lbs | Nutrition, exercise, consistency | General energy |
| 9 | Make furniture (bookshelf) | Woodworking, joinery, finishing | #17 |
| 17 | Forge something | Metalworking, heat treatment | #9 (hardware) |
| 18 | Scale a bouldering wall | Climbing technique, grip strength | #7 |
| 19 | Start a fire (group) | Outdoor skills, fire-making | Social |
| 23 | Create a patio pond | Aquascaping, plumbing, ecology | Outdoor |
| 29 | Build something for Abby (quilting) | Sewing, quilting, design | Craft |

### Cluster F: Social & Gifts
> *Projects that involve or are for other people.*

| # | Project | Key Skills | Feeds Into |
|---|---------|-----------|------------|
| 10 | Hannah & Steven's wedding cookbook | Recipe compilation, layout, printing | #25 (use your font) |
| 13 | Meet someone at Open Sauce | Networking, project demos | #8 (exposure) |
| 24 | Solve Hunt a Killer mystery | Puzzle-solving, deduction | Social |

### Cluster G: Learning & Language
> *Skill acquisition for its own sake.*

| # | Project | Key Skills | Feeds Into |
|---|---------|-----------|------------|
| 6 | Get a promotion/raise | Leadership, visibility, delivery | Career |
| 21 | Learn Japanese (beginner) | Language learning, spaced repetition | #5 (game localization?) |

---

## 2. Cluster Deep-Dives

### Cluster A: Software & AI Core — Detailed Execution

This is your highest-leverage cluster. Every project here builds portfolio, skills,
and directly supports the startup concept.

#### Project #1: Build a Deep Learning Model

**Recommended approach:** Build something that serves another project on this list.

**Option A — Handwriting Recognition (serves the task pad project)**
- Train a character recognition model on your own handwriting
- Start with IAM Handwriting Dataset for pre-training, fine-tune on your samples
- Framework: PyTorch + MLX (Apple Silicon native training)
- Deliverable: A model that reads your handwritten task lists

**Option B — Image Style Transfer or Generation (serves #3, #27)**
- Train a style transfer model for your photography or illustration work
- Or build a LoRA for generating bestiary illustrations in a consistent style

**Option C — Gaussian Splatting Pipeline (serves #30)**
- Build the 3D reconstruction pipeline for your house digital twin
- Combines deep learning with practical spatial computing

**Recommendation:** Option A. It directly enables the handwriting task pad,
counts as a genuine deep learning build, and the fine-tuning workflow
teaches you the full ML lifecycle.

**Token spend plan:**
- `[GEN]` Full MLX training pipeline scaffold for handwriting recognition
- `[GEN]` Data collection and augmentation strategy for personal handwriting
- `[GEN]` Model architecture comparison: CNN vs. Transformer for HTR
- `[GEN]` Evaluation metrics and testing framework
- `[GEN]` Export pipeline: MLX → CoreML for on-device inference

---

#### Project #2: Build 3 Tools

**Tool 1 — Personal Library Manager**
- Track books (owned, read, want-to-read), notes, quotes, lending
- Tech: Go backend + SQLite, React frontend or CLI
- Stretch: barcode scanning, Goodreads import, reading stats

**Tool 2 — Personal Finance Tracker**
- Transaction categorization, budget tracking, net worth over time
- Tech: Go or Rust backend, TypeScript frontend
- Stretch: bank CSV import, recurring transaction detection, projections

**Tool 3 — Time/Project Management (overlaps with task pad concept)**
- This IS the handwriting task pad software side
- Task tracking, time blocking, daily planning, review workflows
- Tech: TypeScript + React (or React Native for #12)
- Stretch: integrate with the handwriting recognition from #1

**Token spend plan:**
- `[GEN]` Database schema designs for all three tools
- `[GEN]` API design (OpenAPI specs) for each tool
- `[GEN]` Component architecture for shared UI patterns
- `[TEMPLATE]` Go service skeleton with auth, logging, migrations pre-wired
- `[TEMPLATE]` React + TypeScript project scaffold with design system

---

#### Project #5: Make a Video Game

**Recommended scope:** A small, polished game you can finish in 2–4 weeks.

**Option A — Rust + Bevy (serves Rust mastery, #8 potential)**
- 2D game: roguelike, puzzle, or arena shooter
- Bevy is well-documented and the ECS architecture is genuinely educational
- Could open-source the game or a Bevy plugin extracted from it

**Option B — TypeScript + Phaser or Three.js (serves web skills)**
- Browser-based game, easy to share and demo at Open Sauce (#13)
- Lower friction to get something playable

**Option C — Godot with GDScript (fastest to playable)**
- If the goal is "make a game" and not "practice a language"
- GDScript is Python-like, very fast iteration

**Recommendation:** Option A if you want it to feed your Rust mastery and
potentially #8. Option B if you want a demo-able thing for Open Sauce.

**Token spend plan:**
- `[GEN]` Game design document template (1–2 pages, not a novel)
- `[GEN]` ECS architecture guide for Bevy (or chosen engine)
- `[GEN]` Game loop, state management, and scene transition patterns
- `[GEN]` Asset pipeline setup (sprites, audio, tilemaps)
- `[GEN]` If using Bevy: a tutorial-style walkthrough of core systems

---

#### Project #8: Open-Source Something Useful

**Strategy:** Extract a useful, general-purpose component from one of your other projects.

Best candidates for extraction:
- A Bevy plugin or game utility from #5
- The handwriting recognition model or training pipeline from #1
- A Go/Rust library discovered while building the tools (#2)
- The e-ink display driver or rendering pipeline from #26
- A logseq plugin as a stepping stone to the startup

**Token spend plan:**
- `[GEN]` Open-source project checklist: README, LICENSE, CONTRIBUTING, CI, docs
- `[GEN]` API design review — is the interface clean enough for public use?
- `[GEN]` Documentation: getting started guide, API reference, examples
- `[TEMPLATE]` GitHub repo template with issue templates, PR template, Actions workflows

---

#### Project #12: iPhone App (30 Under 30 Tracker)

**Concept:** An app that tracks your 30 Before 30 progress.

**Two viable approaches:**

**SwiftUI (native):**
- Best UX, access to all iOS APIs (HealthKit for #7, camera for #3)
- Steeper learning curve if you haven't done Swift
- Ships through TestFlight easily

**React Native / Expo:**
- Leverages your TypeScript skills directly
- Faster to build, code shares with web
- Some iOS features harder to access

**Feature ideas:**
- Project cards with status, photos, notes, completion dates
- Timeline view showing progress over the year
- Photo journal per project (integrates with #3)
- Share cards for social media when you complete one
- Widget showing next actionable project

**Token spend plan:**
- `[GEN]` SwiftUI crash course tailored for TypeScript developers
- `[GEN]` App architecture: navigation, state management, persistence
- `[GEN]` UI component library for the app (cards, progress rings, timelines)
- `[GEN]` CoreML integration guide (for handwriting recognition from #1)

---

#### Project #20: Kaggle Competition

**Strategy:** Pick a competition that aligns with skills from #1.

**If you do handwriting recognition for #1:**
- Look for OCR, document understanding, or NLP competitions
- Your fine-tuning experience transfers directly

**If you do gaussian splatting for #1:**
- Look for computer vision or 3D reconstruction competitions

**General approach:**
- Pick a tabular data competition first if you want a quick win (XGBoost still wins these)
- Pick an NLP or CV competition if you want depth

**Token spend plan:**
- `[GEN]` Kaggle competition workflow: EDA → baseline → iteration → ensembling
- `[GEN]` Feature engineering cookbook for tabular data
- `[GEN]` Training pipeline template with cross-validation, logging, experiment tracking
- `[GEN]` Common competition tricks: pseudo-labeling, test-time augmentation, stacking

---

### Cluster B: Hardware & Physical Computing — Detailed Execution

#### Project #26: E-Ink Art Display

**Recommended hardware:**
- **Display:** Waveshare 7.5" e-ink (800×480), black/white or tri-color
- **Controller:** ESP32 (lower power) or Raspberry Pi Zero 2W (more capable)
- **Frame:** 3D printed (#22) or wooden (#9)

**Software stack:**
- Image processing pipeline in Python or Rust
- Daily/hourly refresh with new art (generative, photos, quotes, weather)
- REST API for pushing new images from other devices

**This connects to the task pad concept:** same e-ink technology,
same driver code, same rendering pipeline.

**Token spend plan:**
- `[GEN]` E-ink display driver guide for your chosen hardware
- `[GEN]` Image dithering algorithms for e-ink (Floyd-Steinberg, etc.)
- `[GEN]` Low-power scheduling patterns for ESP32
- `[GEN]` 3D printable frame design (parametric OpenSCAD)

#### Project #28: JoJo Arrow PCB Art Piece

**Workflow:**
1. Design arrow artwork in vector (Inkscape/Illustrator)
2. Import into KiCad as board outline + copper art
3. Add optional LEDs, microcontroller for lighting effects
4. Order from PCBWay or JLCPCB (art boards are cheap)

**Token spend plan:**
- `[GEN]` KiCad PCB art tutorial — importing SVGs, copper layers, silkscreen
- `[GEN]` LED circuit design for edge-lit or surface-mount effects
- `[GEN]` PCB design checklist: DRC, gerber export, manufacturer specs

---

### Cluster C & D: Creative Projects — Detailed Execution

#### Project #25: Create a Font

**Recommended tool:** FontForge (free) or Glyphs Mini (Mac, paid but excellent)

**Minimum viable font:**
- Uppercase A–Z, lowercase a–z, numbers 0–9, basic punctuation
- That's roughly 70–80 glyphs for a usable font

**Stretch:** Use YOUR handwriting as the basis (connects to #1 and the task pad).
You'd be digitizing your handwriting twice — once as a font, once as training data.

**Token spend plan:**
- `[GEN]` Font design fundamentals: anatomy, metrics, kerning, hinting
- `[GEN]` FontForge workflow guide: template → drawing → spacing → export
- `[GEN]` Handwriting-to-font pipeline: scan → vectorize → import → adjust

#### Project #30: Digital Twin (Gaussian Splatting)

**Pipeline:**
1. Capture: video walkthrough of each room (iPhone LiDAR helps enormously)
2. Extract frames: ffmpeg at ~2fps
3. Structure from Motion: COLMAP for camera poses
4. Train: 3D Gaussian Splatting (original repo or nerfstudio)
5. View: web viewer (Three.js + splat renderer) or Unity

**This is a genuine deep learning project and could count for #1 too.**

**Token spend plan:**
- `[GEN]` Capture best practices: lighting, overlap, coverage patterns
- `[GEN]` COLMAP setup and troubleshooting guide
- `[GEN]` 3DGS training pipeline: hyperparameters, quality metrics, iteration
- `[GEN]` Web viewer implementation guide (Three.js gaussian splat renderer)
- `[GEN]` MLX-based gaussian splatting training (Apple Silicon optimized)

---

### Cluster E: Physical Projects — Brief Plans

These are less LLM-dependent but benefit from research and planning artifacts.

| # | Project | Key Action | Token Spend |
|---|---------|-----------|-------------|
| 7 | 220 lbs | `[GEN]` Nutrition plan + workout program tailored to your schedule | Low |
| 9 | Bookshelf | `[GEN]` Joinery options comparison, cut list calculator, finishing guide | Low |
| 17 | Forge something | `[GEN]` Beginner blacksmithing guide: tools needed, first projects, safety | Low |
| 18 | Bouldering | `[GEN]` Beginner progression guide: V0→V2, technique fundamentals | Low |
| 23 | Patio pond | `[GEN]` Container pond design guide: ecosystem balance, plant selection, filtration | Low |
| 29 | Quilting gift | `[GEN]` Quilting frame or tool design that could be 3D printed (#22) | Low |

---

## 3. The Startup Concept: AI-Native Knowledge Graph

### Vision

Logseq/Obsidian/Roam are **document-first tools with AI bolted on.**
The opportunity is an **AI-first tool with documents as the interface.**

### Core Differentiators

**What existing tools do:**
- Store notes you write
- Search notes you wrote
- Maybe summarize or auto-tag

**What an AI-native tool would do:**
- **Infer connections you didn't explicitly make** — surface relationships between ideas
  across notes, conversations, code, bookmarks, and external sources
- **Actively synthesize** — when you add a new note, the system tells you what it
  connects to, what it contradicts, and what gaps it reveals in your thinking
- **Temporal awareness** — understand how your thinking on a topic evolved over time,
  surface when your current view conflicts with a past decision
- **Multi-modal intake** — handwritten notes (via your task pad), voice memos,
  screenshots, code snippets, bookmarks, highlights — all unified
- **Contextual retrieval** — not just "search" but "given what I'm working on right now,
  what from my knowledge base is relevant?"
- **Executable knowledge** — notes that can become tasks, templates, code, or workflows

### Technical Architecture Sketch

```
┌─────────────────────────────────────────────────────────┐
│                    Client Layer                          │
│  ┌──────────┐  ┌──────────┐  ┌──────────┐  ┌─────────┐ │
│  │ Web App  │  │ iOS App  │  │ CLI      │  │ Task Pad│ │
│  │ (React)  │  │ (Swift)  │  │ (Go/Rust)│  │ (HW+SW)│ │
│  └────┬─────┘  └────┬─────┘  └────┬─────┘  └────┬────┘ │
│       └──────────────┴─────────────┴─────────────┘      │
│                         │ API                           │
├─────────────────────────┼───────────────────────────────┤
│                   Core Services                         │
│  ┌──────────────────────┴──────────────────────────┐    │
│  │              API Gateway (Go/Rust)               │    │
│  └──┬──────────┬──────────┬──────────┬─────────────┘    │
│     │          │          │          │                   │
│  ┌──┴───┐  ┌──┴───┐  ┌──┴───┐  ┌──┴──────────────┐   │
│  │Note  │  │Graph │  │Search│  │AI Inference      │   │
│  │Store │  │Engine│  │Index │  │(local+cloud)     │   │
│  │      │  │      │  │      │  │                  │   │
│  │SQLite│  │ Neo4j│  │Meil- │  │MLX local models │   │
│  │/PG   │  │ or   │  │isear-│  │+ cloud fallback │   │
│  │      │  │custom│  │ch    │  │                  │   │
│  └──────┘  └──────┘  └──────┘  └──────────────────┘   │
│                                                         │
│  ┌─────────────────────────────────────────────────┐    │
│  │           Embedding & Inference Pipeline         │    │
│  │  ┌────────────┐  ┌────────────┐  ┌───────────┐  │    │
│  │  │ Embed on   │  │ Classify & │  │ Generate  │  │    │
│  │  │ ingest     │  │ link       │  │ synthesis │  │    │
│  │  └────────────┘  └────────────┘  └───────────┘  │    │
│  └─────────────────────────────────────────────────┘    │
├─────────────────────────────────────────────────────────┤
│                  Data Layer                              │
│  ┌────────────┐  ┌────────────┐  ┌─────────────────┐   │
│  │ Markdown   │  │ Vector     │  │ Knowledge Graph │   │
│  │ files      │  │ store      │  │ (relations,     │   │
│  │ (portable) │  │ (ChromaDB) │  │  inferences)    │   │
│  └────────────┘  └────────────┘  └─────────────────┘   │
└─────────────────────────────────────────────────────────┘
```

### MVP Scope (achievable as a side project)

**V0.1 — Local-first CLI + TUI (4–6 weeks)**
- Markdown note storage (compatible with Obsidian/Logseq vaults)
- Local embedding on ingest (sentence-transformers via MLX)
- Semantic search: "find notes related to [concept]"
- Auto-linking: suggest connections when saving a new note
- Built in Go or Rust for speed, Python for ML pipeline

**V0.2 — Web UI + Active Synthesis (4–6 weeks)**
- React frontend with graph visualization
- "Daily briefing": here's what's relevant to what you're working on
- Contradiction detection: "in March you said X, but this note says Y"
- Multi-modal intake: images, screenshots, bookmarks

**V0.3 — Handwriting Integration (ongoing)**
- Task pad integration via the handwriting recognition model
- Handwritten notes → structured text → embedded → linked

### Token Spend Plan for Startup

- `[GEN]` Competitive analysis: Logseq, Obsidian, Notion, Mem, Reflect — strengths/gaps
- `[GEN]` Product requirements document for V0.1
- `[GEN]` Database schema design: notes, links, embeddings, graph edges
- `[GEN]` Embedding pipeline architecture: chunking, embedding model selection, indexing
- `[GEN]` Local-first sync architecture: CRDTs or OT for eventual multi-device
- `[GEN]` Graph inference algorithms: community detection, path finding, gap analysis
- `[GEN]` MLX model serving guide for on-device embedding and classification

---

## 4. The Handwriting Recognition Task Pad

### Concept

A physical notepad experience (e-ink tablet or repurposed device) where you write
tasks by hand, and the system digitizes, tracks, and syncs them.

### Hardware Options

**Option A — Repurposed E-Reader (lowest cost, fastest start)**
- Use a Kobo or Kindle with custom firmware (KOReader)
- Add a Wacom-compatible stylus layer (Kobo already has this)
- Run recognition on a connected Mac or RPi
- Cost: ~$50–80 for a used Kobo Libra

**Option B — Raspberry Pi + E-Ink Hat (most hackable)**
- Waveshare 10.3" e-ink display with touch
- RPi Zero 2W or RPi 4 as controller
- Custom UI in Python (Pygame/SDL) or Rust (embedded-graphics)
- Cost: ~$150–200

**Option C — reMarkable 2 Integration (best writing experience)**
- reMarkable has the best stylus experience on the market
- Use the reMarkable API / SSH access to extract strokes
- Process on your Mac: stroke data → segmentation → recognition
- Cost: ~$300 for the tablet (but you get a great general note device)

**Option D — iPad + Apple Pencil (most capable, least hackable)**
- Build as an iPad app (connects to #12)
- Use Apple's Vision framework for on-device OCR
- Or use your custom model via CoreML
- Cost: you may already own this

### Software Architecture

```
┌───────────────────────────────────────────┐
│              Input Device                  │
│  (e-ink tablet / iPad / Kobo)             │
│                                            │
│  Captures: stroke data or raster image     │
└──────────────────┬────────────────────────┘
                   │
                   ▼
┌───────────────────────────────────────────┐
│           Pre-Processing                   │
│                                            │
│  1. Stroke smoothing / denoising           │
│  2. Line segmentation (detect rows)        │
│  3. Checkbox detection (the _ or ✓)        │
│  4. Text region extraction                 │
│  5. Normalize for recognition model        │
└──────────────────┬────────────────────────┘
                   │
                   ▼
┌───────────────────────────────────────────┐
│        Handwriting Recognition             │
│                                            │
│  Pre-trained HTR model (TrOCR or custom)   │
│  Fine-tuned on YOUR handwriting samples    │
│  Running locally via MLX / CoreML          │
│                                            │
│  Input: line image                         │
│  Output: text string + confidence score    │
└──────────────────┬────────────────────────┘
                   │
                   ▼
┌───────────────────────────────────────────┐
│          Task Parsing                      │
│                                            │
│  Pattern matching on recognized text:      │
│  "_ task description" → new task           │
│  "✓ task description" → completed task     │
│  "- note text" → note/context              │
│  Indentation → subtask hierarchy           │
│                                            │
│  Small local LLM for ambiguity resolution  │
└──────────────────┬────────────────────────┘
                   │
                   ▼
┌───────────────────────────────────────────┐
│        Sync & Storage                      │
│                                            │
│  Local SQLite database                     │
│  Sync to task management tool (#2)         │
│  Optional: sync to logseq/obsidian/startup│
│  API for querying tasks from any device    │
└───────────────────────────────────────────┘
```

### Fine-Tuning on Your Handwriting

**Data collection strategy:**
1. Write out 50–100 task list pages using your normal handwriting
2. Photograph or scan each page (iPhone camera is fine)
3. Manually transcribe each line (this is the tedious but essential step)
4. Split into training (80%) and validation (20%) sets
5. Augment: slight rotations, noise, contrast variations, ink thickness

**Model approach:**
- Start with TrOCR (Transformer-based OCR) pre-trained on IAM dataset
- Fine-tune on your handwriting samples using MLX or PyTorch
- Target: >90% character accuracy on your writing specifically
- The model doesn't need to generalize — it only needs to read YOUR handwriting

**Incremental deployment:**
- **Week 1–2:** Collect handwriting samples, build data pipeline
- **Week 3–4:** Fine-tune model, evaluate accuracy
- **Week 5–6:** Build the task parsing layer
- **Week 7–8:** Hardware integration (whichever option you chose)
- **Ongoing:** Active learning — correct misrecognitions, retrain periodically

### Token Spend Plan

- `[GEN]` TrOCR fine-tuning pipeline for MLX on Apple Silicon
- `[GEN]` Data augmentation strategies for handwriting data
- `[GEN]` Line segmentation algorithms (projection profile, connected component)
- `[GEN]` Checkbox/bullet detection using simple CV (no ML needed)
- `[GEN]` Task parsing grammar and pattern matching rules
- `[GEN]` CoreML export pipeline from PyTorch/MLX
- `[GEN]` E-ink display driver and touch input handling (for hardware option B)

---

## 5. Token Spend Strategy

### Budget Allocation Across All Projects

Given your April–June window with the old Copilot pricing, here's how to allocate:

| Priority | Category | % Budget | What You're Generating |
|----------|----------|----------|----------------------|
| 1 | Knowledge base (prior plan) | 25% | Reusable patterns, idioms, architecture guides |
| 2 | Startup architecture & design | 20% | PRDs, schemas, pipeline designs, competitive analysis |
| 3 | Handwriting/ML pipeline | 15% | Training pipelines, model architecture, data strategies |
| 4 | Tool scaffolding (#2, #12) | 15% | API specs, database schemas, component libraries |
| 5 | Game + creative projects | 10% | Game design docs, engine guides, creative tooling |
| 6 | Hardware project research | 10% | PCB guides, e-ink drivers, 3D model templates |
| 7 | Physical project planning | 5% | Woodworking plans, forging guides, pond design |

### High-Value Prompt Patterns

These prompt structures extract maximum value per token:

**The "Teach Me to Fish" Prompt:**
> "Don't just write the code. Explain the architectural decision, the alternatives
> you considered, why this approach wins for my constraints, and what I'd need
> to change if my constraints shifted. Then write the code."

**The "Rosetta Stone" Prompt:**
> "Show me idiomatic implementations of [pattern] in Go, Rust, TypeScript,
> and Python. For each, explain what makes it idiomatic in that language
> and what a newcomer would get wrong."

**The "Decision Framework" Prompt:**
> "I need to choose between [X, Y, Z] for [use case]. Create a decision matrix
> with weighted criteria relevant to my constraints: [list constraints].
> Make a recommendation but show your reasoning."

**The "Extract and Generalize" Prompt:**
> "Here's code from my project: [paste]. Extract the general pattern, document
> it as a reusable template with customization points clearly marked,
> and add it to my knowledge base format."

**The "Pre-Mortem" Prompt:**
> "I'm about to build [X] with [Y approach]. Assume it's 6 months from now
> and it failed. What went wrong? What would I wish I'd done differently?
> What are the top 3 risks and mitigations?"

---

## 6. Master Timeline

### Priority Tiers

**Tier 1 — Start Immediately (high overlap, high LLM leverage)**
- Knowledge base setup (Phase 0 of prior plan)
- Handwriting data collection (just start writing task lists and scanning)
- E-ink display PoC (#26) — this feeds the task pad AND is a standalone win
- Deep learning model (#1) — the handwriting recognition model
- Tool #3 time/project management (#2) — the software side of the task pad

**Tier 2 — Start After Foundation (builds on Tier 1)**
- Startup MVP V0.1 — CLI + semantic search over your knowledge base
- iPhone app (#12) — tracker app, possibly React Native
- Video game (#5) — pick engine, scope ruthlessly
- Kaggle competition (#20) — use skills from #1
- Open-source extraction (#8) — pull from whatever's most polished

**Tier 3 — Parallel / Low-Dependency (do when energy/time allows)**
- Physical projects: bookshelf, forging, bouldering, patio pond
- Creative projects: font, bestiary, song, short fiction
- Social projects: cookbook, streamer wheel, Hunt a Killer
- Photography: ongoing, no blocking dependencies
- Japanese: daily practice, no blocking dependencies

**Tier 4 — Capstone / Late**
- Digital twin (#30) — benefits from all ML experience gained earlier
- JoJo PCB (#28) — after e-ink and hardware experience
- Quilting gift for Abby (#29) — whenever inspiration strikes
- 3D print custom model (#22) — anytime, but more interesting after CAD skills from other projects

### Suggested Monthly Focus

| Month | Primary Focus | Secondary | Ongoing |
|-------|--------------|-----------|---------|
| **Now–May** | Knowledge base generation, handwriting data collection, e-ink PoC | Copilot token generation sprint | Weight loss, Japanese, photography |
| **Jun** | Deep learning model (handwriting), tool #3 MVP | Game design doc + prototype | Bouldering, reading |
| **Jul** | Task pad integration, startup V0.1 CLI | Video game development | Physical projects |
| **Aug** | Startup V0.2 web UI, iPhone app | Kaggle competition | Font, creative writing |
| **Sep** | Open-source release, polish + ship | PCB design, 3D printing | Bestiary, song |
| **Oct** | Capstone projects: digital twin, furniture | Cookbook, streamer wheel | Fire, Hunt a Killer |
| **Nov–Dec** | Polish, ship remaining items, startup V0.3 | Quilting gift, patio pond | Wrap up stragglers |

---

## 7. Prompt Library

### Project Kickoff Prompts

**For any new project:**
```
I'm starting project: [name]
Goal: [one sentence]
Constraints: [time, budget, skills, hardware]
Languages/tools I want to use: [list]
Related projects it should integrate with: [list]

Give me:
1. A scoped MVP definition (what's the smallest thing that counts?)
2. A technical architecture overview
3. A prioritized task list for the first 2 weeks
4. The 3 biggest risks and how to mitigate them
5. What I should generate/document now while LLM tokens are cheap
```

**For deep learning projects:**
```
I'm building a [model type] for [use case].
Hardware: Apple M-series Mac with [RAM]GB
Framework preference: MLX > PyTorch > TensorFlow
Dataset: [describe or link]

Give me:
1. Model architecture recommendation with rationale
2. Training pipeline code (data loading, augmentation, training loop, evaluation)
3. Hyperparameter starting points and tuning strategy
4. MLX-specific optimizations for Apple Silicon
5. Export path to CoreML for on-device inference
```

**For hardware projects:**
```
I'm building a [hardware project].
Components I'm considering: [list]
Budget: [amount]
Skills I have: [soldering/PCB design/3D printing/etc.]
Skills I'm learning: [list]

Give me:
1. Bill of materials with specific part numbers and sources
2. Schematic or wiring diagram
3. Software/firmware architecture
4. Build sequence (what order to assemble and test)
5. Common failure modes and how to debug them
```

### Knowledge Extraction Prompts

**After completing any task:**
```
I just finished implementing [what].
Here's the code/approach: [paste or describe]

Extract for my knowledge base:
1. The general pattern (language-agnostic)
2. Language-specific idiom notes
3. Gotchas I should remember
4. When I'd use this pattern again vs. alternatives
5. Format as a knowledge base entry using my pattern template
```

**For architectural decisions:**
```
I chose [X] over [Y] for [reason] in my [project].

Write an ADR (Architecture Decision Record) covering:
1. Context: what forces led to this decision
2. Decision: what we chose and why
3. Consequences: positive, negative, and neutral
4. Alternatives considered with tradeoff analysis
```

---

## 8. Appendices

### Appendix A: Project Dependency Graph

```
#1 (DL model) ──────► #20 (Kaggle)
     │
     ▼
Task Pad (HW) ◄──── #26 (e-ink) ◄──── #22 (3D print frame)
     │
     ▼
#2 Tool #3 (task mgmt) ──► #12 (iPhone app)
     │
     ▼
Startup V0.1 ──► V0.2 ──► V0.3
     ▲
     │
Knowledge Base (prior plan)

#5 (game) ──► #8 (open source) ──► #13 (Open Sauce demo)
                                        │
#15 (song) ──► #5 (game audio)         ▼
#16 (fiction) ► #27 (bestiary)         #6 (promotion)
#25 (font) ──► #10 (cookbook)
#9 (furniture) ◄── #17 (forge)
#28 (PCB) ◄──── #11 (streamer wheel)
#30 (digital twin) ◄── #1 (DL model, gaussian splatting path)
```

### Appendix B: Skills Acquisition Map

| Skill | Learned From | Applied In |
|-------|-------------|------------|
| MLX / PyTorch training | #1, #20 | Task pad, startup, #30 |
| E-ink display driving | #26 | Task pad |
| CoreML export | #1 | #12, task pad |
| PCB design (KiCad) | #28 | #11, future hardware |
| 3D modeling (CAD) | #22 | #26 frame, #9, #29 |
| Game engine (Bevy/etc) | #5 | #8 (open source plugin) |
| React Native / SwiftUI | #12 | Startup mobile client |
| Embedding pipelines | Startup | Knowledge base search |
| Go service architecture | #2 | Startup backend, #8 |
| Rust systems programming | #5, #26 | Startup CLI, #8 |
| Typography / font design | #25 | #10 cookbook, #27 bestiary |
| Creative writing | #16 | #27 bestiary lore |
| Music production | #15 | #5 game audio |

### Appendix C: Quick Win Stack Ranking

If you want to knock out items fast to build momentum, here they are ranked
by effort-to-completion (assuming you start today):

| Rank | Project | Est. Time | Why It's Fast |
|------|---------|-----------|---------------|
| 1 | #19 Start a fire | 1 day | Gather friends, go outside, do it |
| 2 | #18 Scale a bouldering wall | 1–3 sessions | Go to a gym, try V0 routes |
| 3 | #24 Hunt a Killer | 1–3 evenings | Sit down and solve it |
| 4 | #22 3D print a model | 2–5 days | Design or download, slice, print |
| 5 | #16 Short fiction | 1–2 weeks | Write it. LLM can help with editing. |
| 6 | #15 Make a song | 1–2 weeks | GarageBand/Logic, keep it simple |
| 7 | #25 Create a font | 2–3 weeks | 70 glyphs, systematic process |
| 8 | #23 Patio pond | 1–2 weekends | Container, pump, plants, fish |
| 9 | #3 10 photos | Ongoing | Start shooting intentionally |
| 10 | #10 Cookbook | 2–4 weeks | Collect recipes, design, print |