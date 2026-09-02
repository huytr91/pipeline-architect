# Pipeline Architect

**Measure pipelines on your machine. Compare architectures. Share evidence. Build on a standard schema.**

Pipeline Architect is an **open-core system for empirical AI pipeline architecture**.

It is not simply “ask an LLM which model or OCR library to use.”

Instead, Pipeline Architect:

1. **Interviews the user** to understand workload, constraints, and hardware.
2. **Researches existing solutions and public benchmark evidence** (where available) to establish a prior.
3. **Generates competing pipeline architectures** rather than committing to the first plausible solution.
4. **Runs candidates locally** on the user's actual machine and workload.
5. **Measures and compares** latency, throughput, memory, quality, cost, and reliability.
6. Produces a ranked set of **architecture recommendations backed by empirical evidence**.
7. Packages the selected architecture into a structured **Solution Pipeline Packet** for downstream implementation tools, including coding agents and RPA platforms.

> **Prior is a recommendation. Measurement is evidence.**

Public benchmarks and research provide the initial prior. Local experiments turn that prior into evidence. As more users voluntarily contribute anonymized benchmark observations, the system can learn which architectural decisions perform best across different workloads, hardware, and runtime environments.

> **Architect first. Code second.**

Pipeline Architect determines *what* should be built and *why*. Coding agents and RPA platforms then implement the chosen architecture.

---

## The gap

Benchmarks like MLPerf answer: *"How fast is this model?"*  
Few tools answer: *"For **this problem** on **this hardware**, which **end-to-end pipeline** should I build — and what's the evidence?"*

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

The open repos are useful **on their own** — run benchmarks locally, no API keys required.

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

---

## Solution Pipeline Packet

Handoff format for agents and RPA builders: `solution_graph` + `business_slots` + `open_items` + `integrations` (OAuth specs, no tokens).

Example: [email + keyword SB123 + OCR slot](https://github.com/huytr91/pa-schema/tree/main/examples/email-sb123)

---

## Open core boundary

**Open:** methodology, pipeline schema, benchmark protocol, harness, adapters, contribution format.

**Commercial / hosted intelligence** (not in public repos): architecture ranking, performance prediction, mutation, failure patterns, hardware–workload correlations, experiment selection.

Methodology is transparent. The decision engine evolves from accumulated empirical evidence.

> Show the evidence. Hide the recipe.

---

## Community contributions

Benchmark contributions are **pipeline metrics only** — latency, RAM, quality, structural fingerprints.  
**Not** used by interview or architecture agents. See [contribution privacy](https://github.com/huytr91/pa-schema/blob/main/docs/contribution-privacy.md).

```bash
python cli.py contribute export --db benchmarks.duckdb --experiment-id <id> \
  --problem problem.yaml --pipelines pipelines/a.yaml --out contribution.json \
  --i-agree-to-terms
```

---

## Status

| Component | State |
|-----------|--------|
| pa-harness V0 | ✅ Runnable (mock adapters; real OCR PRs welcome) |
| pa-schema v1.1 | ✅ Solution Pipeline Packet + examples |
| pa-adapters | ✅ Mock reference adapters |
| Contribution export | ✅ CLI (upload API planned) |
| Leaderboard / central API | 🚧 Planned |
| Full product (steps 1–3, 6) | 🔒 Private preview |

## Docs

- [Observation schema](https://github.com/huytr91/pa-schema/blob/main/docs/observation.md)
- [Solution Pipeline Packet](https://github.com/huytr91/pa-schema/blob/main/docs/solution-pipeline-packet.md)
- [Contribution privacy](https://github.com/huytr91/pa-schema/blob/main/docs/contribution-privacy.md)

## License

MIT for `pa-schema`, `pa-harness`, `pa-adapters`.
