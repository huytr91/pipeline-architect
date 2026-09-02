# Pipeline Architect

**Measure pipelines on your machine. Compare architectures. Share evidence. Build on a standard schema.**

Open-core · MIT · runs locally · no API key required for the harness.

---

## The problem isn't always the code

Have you ever built an AI product by trial and error — swap the model, try another OCR library, tweak the prompt — and still get results that are *almost* right?

Example: **PDF → Word**, but tables misalign, fonts break, images land in the wrong place.

Sometimes the issue isn't your **code**. It's your **pipeline** — which steps you chain, in what order, with what trade-offs on *your* hardware.

**Pipeline Architect** shortens that loop:

**Understand the problem → design the pipeline → benchmark on your machine → pick the best approach → export a [Solution Pipeline Packet](https://github.com/huytr91/pa-schema/tree/main/examples/email-sb123) to start coding.**

> **Design the pipeline first. Code second.**

> **Prior is a recommendation. Measurement is evidence.**

Coding agents and RPA platforms implement the architecture; Pipeline Architect helps you decide *what* to build and *why* — with local measurements, not vibes alone.

---

## What it does (full system)

Interview → research public evidence → propose competing architectures → **run candidates locally** → measure latency, memory, quality, cost → rank with evidence → package the choice for downstream tools.

Steps 1–3 and ranking run in the **private / commercial layer** today. The **open repos** below let you benchmark and use the schema **standalone**.

---

## Multi-domain (not PDF/OCR only)

Document AI, **ASR**, **vision**, RAG, and more. A **Solution Pipeline** mixes design nodes (email, rules, notify) with **`ai_pipeline_slot`** nodes — each slot has its own domain and measured sub-pipelines.

OCR shows up often in docs as the **first reference vertical** (V0), not a product limit.

| Domain | Harness example | Role |
|--------|-----------------|------|
| `document-ocr` | `problem.example.yaml` | Reference vertical + email/SB123 packet |
| `audio-transcription` | `problem.audio.example.yaml` | ASR / transcription slots |
| `image-classification` | `problem.image.example.yaml` | Vision / classification slots |

Details: [multi-domain](https://github.com/huytr91/pa-schema/blob/main/docs/multi-domain.md)

---

## The gap

MLPerf-style benchmarks ask: *"How fast is this model?"*  
Pipeline Architect asks: *"For **this problem** on **this hardware**, which **end-to-end pipeline** should I build — and what's the evidence?"*

```
Problem Fingerprint × Pipeline × Hardware → observations → ranked candidates
                                                    ↓
                          Solution graph + AI slots + export packet
```

---

## What's open today (MIT)

| Layer | Status | Repos |
|-------|--------|-------|
| Schema & handoff format | ✅ Public | [pa-schema](https://github.com/huytr91/pa-schema) |
| Local benchmark harness | ✅ Public | [pa-harness](https://github.com/huytr91/pa-harness) |
| Component adapters | ✅ Public | [pa-adapters](https://github.com/huytr91/pa-adapters) |
| Interview agent | 🔒 Private / preview | — |
| Architecture generation & ranking | 🔒 Commercial layer | — |
| Consulting web UI | 🔒 Private / preview | — |

### Quick start

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

Structured handoff for agents and RPA: `solution_graph` + `business_slots` + `open_items` + `integrations` (OAuth specs, no tokens).

Example: [email + keyword SB123 + OCR slot](https://github.com/huytr91/pa-schema/tree/main/examples/email-sb123) — *one reference workflow; slots can use other domains.*

---

## Open core boundary

**Open:** methodology, schema, benchmark protocol, harness, adapters, contribution format.

**Commercial / hosted:** ranking engine, prediction, mutation, failure patterns, experiment selection.

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
| Full product (interview + rank) | 🔒 Private preview |

## Docs

- [Multi-domain model](https://github.com/huytr91/pa-schema/blob/main/docs/multi-domain.md)
- [Observation schema](https://github.com/huytr91/pa-schema/blob/main/docs/observation.md)
- [Solution Pipeline Packet](https://github.com/huytr91/pa-schema/blob/main/docs/solution-pipeline-packet.md)
- [Contribution privacy](https://github.com/huytr91/pa-schema/blob/main/docs/contribution-privacy.md)

## License

MIT for `pa-schema`, `pa-harness`, `pa-adapters`.
