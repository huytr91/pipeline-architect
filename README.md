# Pipeline Architect

**Measure pipelines on your machine. Share evidence. Build on a standard schema.**

Pipeline Architect is an **open-core** project for empirical AI pipeline architecture:
not "ask an LLM which OCR to use", but **run candidates locally**, record observations,
and hand off a structured **Solution Pipeline Packet** to implementation tools (RPA, coding agents).

> **Show the evidence. Hide the recipe.**

## The gap

Benchmarks like MLPerf answer: *"How fast is this model?"*  
Few tools answer: *"For **this problem** on **this hardware**, which **end-to-end pipeline**
should I build — and what's the evidence?"*

```
Problem Fingerprint × Pipeline × Hardware → observations → ranked candidates
                                                    ↓
                          Solution graph + AI slots + export packet
```

## Open source (MIT)

| Repo | Description |
|------|-------------|
| [**pa-schema**](https://github.com/huytr91/pa-schema) | JSON Schema: Observation, Problem Fingerprint, Solution Pipeline Packet v1.1 |
| [**pa-harness**](https://github.com/huytr91/pa-harness) | CLI: benchmark N≥3 runs, latency/RAM, Bradley-Terry quality, DuckDB |
| [**pa-adapters**](https://github.com/huytr91/pa-adapters) | Component wrappers — mock builtins + PRs for PaddleOCR, Tesseract, … |

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

**No API keys** for the harness.

## Solution Pipeline Packet

Handoff format for agents and RPA builders: `solution_graph` + `business_slots` +
`open_items` + `integrations` (OAuth specs, no tokens).

Example: [email + keyword SB123 + OCR slot](https://github.com/huytr91/pa-schema/tree/main/examples/email-sb123)

**Community benchmark contributions** are pipeline metrics only — [not used by AI agents](https://github.com/huytr91/pa-schema/blob/main/docs/contribution-privacy.md).

## Not open source

- Architecture Agent (LLM candidate generation, ranking engine)
- Evidence hierarchy weights & belief update
- Full consulting web UI

The harness and schema are useful **without** the commercial agent layer.

## Status

| Component | State |
|-----------|--------|
| pa-harness V0 | ✅ Runnable (mock adapters) |
| pa-schema v1.1 | ✅ Solution Pipeline Packet + example |
| pa-adapters | ✅ Mock reference; real OCR adapters welcome |
| Leaderboard / central API | 🚧 Planned |

## Docs

- [Observation schema](https://github.com/huytr91/pa-schema/blob/main/docs/observation.md)
- [Solution Pipeline Packet](https://github.com/huytr91/pa-schema/blob/main/docs/solution-pipeline-packet.md)

## License

MIT for `pa-schema`, `pa-harness`, `pa-adapters`.
