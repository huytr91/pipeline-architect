# Pipeline Architect

### Don't let AI lock you into the first pipeline it thinks of.

Open-core · MIT · [multi-domain](https://github.com/huytr91/pa-schema/blob/main/docs/multi-domain.md) · harness runs locally (no API key)

When you ask an AI assistant how to solve a complex AI workload, it often gives you **one pipeline that sounds right**.

It may be reasonable. It may even be the best choice.  
But you usually don't know that yet — and once you start building, changing direction gets expensive.

**Pipeline Architect takes a different approach:** explore the problem, then surface **3 viable pipeline candidates** so you can compare before you commit.

> **Don't ask AI for the pipeline. Ask it to show you the possibilities.**

---

## The problem

You ask:

> *"How should I build a system that turns scanned PDFs into structured Excel files?"*

A typical assistant answers:

```text
PDF → OCR → LLM → Excel
```

Looks good. You build it. Then OCR fails on your docs, tables collapse, or the LLM invents cells.

The AI wasn't necessarily "wrong".  
**It committed to one approach before you explored the alternatives.**

---

## Three candidates, not one answer

Pipeline Architect treats architecture as a **choice between plausible approaches**.

After understanding your workload and constraints, it presents **3 viable architectures** — for example:

```text
1. Reliable        OCR → Layout Parser → Validator
                   Simple, predictable

2. Quality-first   OCR → Vision → LLM → Validator
                   Better for difficult documents

3. Lightweight     Local OCR → Rules → Excel
                   Lower cost, local processing
```

These aren't *the* answer. They're **candidates worth comparing**.  
You pick the direction that matches your priorities (quality, speed, cost, privacy, simplicity).

### Typical AI workflow

```text
Problem → AI reasoning → One recommendation → Build
```

### Pipeline Architect

```text
Problem → Interview → Constraints → Explore alternatives
              ↓
     ┌─────────┬─────────┬─────────┐
     │ Option 1│ Option 2│ Option 3│
     └─────────┴─────────┴─────────┘
              ↓
         You decide → measure on your machine → commit
```

**The goal isn't to replace your judgment** — it's to give you **more than one reasonable direction before you lock in.**

> Prior is a recommendation. Measurement is evidence.  
> Design the pipeline first. Code second.

Generated pipelines are **hypotheses**, not automatic optima. Validate them on your real workload before shipping.

---

## Multi-domain (not PDF/OCR only)

Document AI, **ASR**, **vision**, RAG, and more. OCR appears in examples as the **first reference vertical**, not a product limit.

| Domain | Harness example |
|--------|-----------------|
| `document-ocr` | `problem.example.yaml` |
| `audio-transcription` | `problem.audio.example.yaml` |
| `image-classification` | `problem.image.example.yaml` |

Details: [multi-domain](https://github.com/huytr91/pa-schema/blob/main/docs/multi-domain.md)

---

## What's open today (MIT)

| Layer | Status | Repos |
|-------|--------|-------|
| Schema & handoff format | ✅ Public | [pa-schema](https://github.com/huytr91/pa-schema) |
| Local benchmark harness | ✅ Public | [pa-harness](https://github.com/huytr91/pa-harness) |
| Component adapters | ✅ Public | [pa-adapters](https://github.com/huytr91/pa-adapters) |
| Interview + TOP 3 candidates | 🔒 Private / preview | — |
| Ranking / prediction engine | 🔒 Commercial layer | — |
| Consulting web UI | 🔒 Private / preview | — |

**Open today:** measure candidates locally and use a standard Solution Pipeline Packet.  
**Full product:** interview → 3 candidates → evidence ranking (preview / commercial).

### Quick start (harness)

```bash
git clone https://github.com/huytr91/pa-harness.git
cd pa-harness
python -m venv .venv && source .venv/bin/activate   # Windows: .venv\Scripts\activate
pip install -r requirements.txt

python cli.py run \
  --problem problem.example.yaml \
  --pipelines pipelines/vn-ocr-native-fallback-v1.yaml pipelines/vn-ocr-baseline-v1.yaml \
  --samples-dir samples --db benchmarks.duckdb --runs 5
```

Feedback, real-world benchmarks, and adapter PRs (Whisper, PaddleOCR, …) are welcome.

---

## Solution Pipeline Packet

After you choose a direction, export a structured handoff for coding agents and RPA:  
`solution_graph` + `business_slots` + `open_items` + `integrations` (OAuth specs, no tokens).

Example: [email + keyword SB123 + OCR slot](https://github.com/huytr91/pa-schema/tree/main/examples/email-sb123)

---

## Open core boundary

**Open:** methodology, schema, benchmark protocol, harness, adapters, contribution format.

**Commercial / hosted:** ranking weights, prediction, mutation, failure patterns, experiment selection.

> Show the evidence. Hide the recipe.

---

## Community contributions

Pipeline **metrics only** (latency, RAM, quality) — not used by interview or architecture agents.  
[Contribution privacy](https://github.com/huytr91/pa-schema/blob/main/docs/contribution-privacy.md)

```bash
python cli.py contribute export --db benchmarks.duckdb --experiment-id <id> \
  --problem problem.yaml --pipelines pipelines/a.yaml --out contribution.json \
  --i-agree-to-terms
```

---

## Status

| Component | State |
|-----------|--------|
| pa-harness V0 | ✅ Runnable (mock adapters; any domain) |
| pa-schema v1.1 | ✅ Solution Pipeline Packet + examples |
| pa-adapters | ✅ Mock reference; real adapter PRs welcome |
| Contribution export | ✅ CLI (upload API planned) |
| Leaderboard / central API | 🚧 Planned |
| Interview + TOP 3 product UX | 🔒 Private preview |

## Docs

- [Multi-domain model](https://github.com/huytr91/pa-schema/blob/main/docs/multi-domain.md)
- [Observation schema](https://github.com/huytr91/pa-schema/blob/main/docs/observation.md)
- [Solution Pipeline Packet](https://github.com/huytr91/pa-schema/blob/main/docs/solution-pipeline-packet.md)
- [Contribution privacy](https://github.com/huytr91/pa-schema/blob/main/docs/contribution-privacy.md)

## License

MIT for `pa-schema`, `pa-harness`, `pa-adapters`.
