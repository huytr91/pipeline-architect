# Pipeline Architect

### AI can design your pipeline. Your machine should decide if it works.

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](https://opensource.org/licenses/MIT)
[![PRs welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](https://github.com/huytr91/pa-adapters/issues)
[![good first issue](https://img.shields.io/badge/good%20first%20issue-welcome-1f6feb.svg)](https://github.com/huytr91/pa-adapters/labels/good%20first%20issue)

**Don't build the wrong AI pipeline. Measure it first.**

AI can recommend a pipeline.  
It cannot know how that pipeline will behave on **your machine**, with **your workload**, until you run it.

Pipeline Architect turns architecture decisions into **measurable experiments**.

```text
Your workload
     ↓
AI + public evidence
     ↓
3 candidate pipelines
     ↓
Run them locally
     ↓
Measure latency · memory · quality · cost
     ↓
Evidence-backed decision
```

**Prior is a recommendation. Measurement is evidence.**

Open-core · MIT · [multi-domain](https://github.com/huytr91/pa-schema/blob/main/docs/multi-domain.md) · local harness (no API key)

---

## The problem

You ask an AI:

> "What's the best OCR pipeline for my 40-page Vietnamese PDFs?"

It gives you:

```text
PDF → OCR → Layout → LLM → DOCX
```

Looks reasonable. So you build it.

Then you discover:

- It takes **8 minutes** instead of 40 seconds
- RAM hits **14 GB**
- Vietnamese quality is poor on *your* scans
- Parallelism makes it slower
- Your laptop behaves nothing like the benchmark machine
- The "best model" isn't the best **pipeline** for your workload

The architecture was never measured.  
You just spent days implementing a **guess**.

---

## What Pipeline Architect does

Instead of asking:

> "What architecture should I build?"

ask:

> "Which architectures should I test — and what does the evidence say?"

Pipeline Architect:

1. Interviews your workload
2. Researches public benchmarks and prior evidence
3. Generates **3 candidate pipelines**
4. Runs them on **your** machine
5. Measures the **same** workload
6. Compares results
7. Produces an **evidence-backed** Solution Pipeline Packet

No benchmark? No measurement?  
Then the system **does not pretend to know**.

Missing details → we ask.  
Unmeasured numbers are labeled **estimates** — never as “measured.”  
Unattended production stays **false** until a real local run (`run_count > 0`).

---

## The important distinction

Most AI architecture tools stop here:

```text
LLM → Recommendation → "Use Pipeline A"
```

Pipeline Architect goes further:

```text
LLM
 ↓
Candidate A ─┐
Candidate B ─┼──→ LOCAL EXPERIMENT
Candidate C ─┘
               ↓
          REAL MEASUREMENTS
               ↓
     Evidence-backed decision
```

The goal isn't to make the AI sound confident.  
The goal is to make the decision **testable**.

---

## Tiny example

Workload:

```yaml
workload:
  type: scanned_pdf
  pages: 42
  language: vi
  output: docx

constraints:
  max_ram: 16GB
  cpu_only: true
  local_only: true
```

Candidates might look like:

```text
A  PDF → PaddleOCR → reconstruct
B  PDF → OCR → layout → reconstruct
C  PDF → vision-language → structured doc
```

You run the **same documents** through all three. Then you choose from evidence — not vibes.

| Pipeline | Latency | RAM | Quality* | Cost |
|----------|---------|-----|----------|------|
| A | 4m 12s | 3.1 GB | 82% | $0 |
| B | 6m 48s | 5.7 GB | 91% | $0 |
| C | 2m 31s | 9.8 GB | 89% | $0 |

\*Illustrative. Real numbers come from **your** run.

Now you're not choosing because “the AI said B.”  
You're choosing because **B performed this way on your workload and hardware.**

Real L2 ops beachhead (metrics only, no PDFs):  
[pa-harness / results/sample-ocr-vi](https://github.com/huytr91/pa-harness/tree/main/results/sample-ocr-vi)

---

## Prior ≠ Measured

A model card says:

> "Model X achieved 92% on dataset Y."

That's **prior** evidence.

It does **not** mean:

> "Model X will achieve 92% on your documents, on your CPU, with your pipeline."

```text
PUBLIC PRIOR
    ↓
Candidate architecture
    ↓
LOCAL EXPERIMENT
    ↓
MEASURED EVIDENCE
```

Prior informs the experiment.  
Measurement informs the decision.

### Evidence-gated recommendations

Before a local run:

```json
{
  "recommendation": null,
  "evidence": "estimated",
  "run_count": 0
}
```

After benchmarking:

```json
{
  "recommendation": "pipeline_b",
  "evidence": "measured",
  "run_count": 3
}
```

**"I think this should work" ≠ "we measured that it works."**

---

## Why this exists

AI made architecture cheap. You can invent ten plausible designs in seconds.  
Running the wrong one is still expensive.

The bottleneck moved from:

> "Can I design an architecture?"

to:

> "Which architecture should I actually trust?"

Pipeline Architect makes that decision **empirical**.

**Bring the experiment to the data — not the data to the benchmark.**

---

## What's open today (MIT)

| Layer | Status | Repo |
|-------|--------|------|
| Schema & Solution Pipeline Packet | Public | [pa-schema](https://github.com/huytr91/pa-schema) |
| Local benchmark harness | Public | [pa-harness](https://github.com/huytr91/pa-harness) |
| Component adapters | Public | [pa-adapters](https://github.com/huytr91/pa-adapters) |
| Interview + TOP 3 candidates | Private / preview | — |
| Ranking / Fit Score engine | Private / preview | — |
| Consulting web UI | Private / preview | — |

**Open today:** measure candidates locally + standard packet format.  
**Private preview:** interview → candidates → optional measure → evidence-gated export.

Domains: OCR, ASR, vision, and more — OCR is the **first vertical**, not a hard limit.  
See [multi-domain](https://github.com/huytr91/pa-schema/blob/main/docs/multi-domain.md).

---

## Quick start (harness)

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

Describe a real workload later:

```text
40-page Vietnamese scanned PDFs → editable DOCX
16 GB RAM · CPU only · local · preserve tables
```

Candidates + experiment plan beat one confident guess.

---

## The output: Solution Pipeline Packet

Not another chat reply — a reproducible record of:

- Workload & constraints  
- Candidates & prior evidence  
- Experiment config & measurements  
- Trade-offs & decision  

So the question becomes:

> "Why did we choose this pipeline?"

not:

> "Because an LLM recommended it."

Example packet: [email + SB123 + OCR](https://github.com/huytr91/pa-schema/tree/main/examples/email-sb123)

---

## Good first issues

| Repo | Easy wins |
|------|-----------|
| [pa-adapters](https://github.com/huytr91/pa-adapters/labels/good%20first%20issue) | Real adapters (PaddleOCR, Whisper, …) |
| [pa-schema](https://github.com/huytr91/pa-schema/labels/good%20first%20issue) | Docs, examples, clearer schema comments |
| [pa-harness](https://github.com/huytr91/pa-harness/labels/good%20first%20issue) | Sample problems, CLI help, tests |

---

## Philosophy

1. **Design before code.** Don't spend three days implementing what an LLM invented in three seconds.  
2. **Benchmark the pipeline, not just the model.** A model is one component.  
3. **Your hardware matters.** A leaderboard machine isn't your machine.  
4. **Your workload matters.** A dataset isn't your document.  
5. **Uncertainty is information.** If it hasn't been measured, say so.

---

## In one sentence

**Pipeline Architect helps you compare AI pipelines before you build the wrong one.**

AI proposes.  
Your machine measures.  
Evidence decides.

---

## Contributing & privacy

Interesting work isn't more confident recommendations — it's a common loop:

**describe → run → measure → compare → reproduce**

Metrics contribution (no document bytes):  
[contribution privacy](https://github.com/huytr91/pa-schema/blob/main/docs/contribution-privacy.md)

```bash
python cli.py contribute export --db benchmarks.duckdb --experiment-id <id> \
  --problem problem.yaml --pipelines pipelines/a.yaml --out contribution.json \
  --i-agree-to-terms
```

## Docs

- [Multi-domain](https://github.com/huytr91/pa-schema/blob/main/docs/multi-domain.md)
- [Observation schema](https://github.com/huytr91/pa-schema/blob/main/docs/observation.md)
- [Solution Pipeline Packet](https://github.com/huytr91/pa-schema/blob/main/docs/solution-pipeline-packet.md)
- [Contribution privacy](https://github.com/huytr91/pa-schema/blob/main/docs/contribution-privacy.md)

## License

MIT for `pa-schema`, `pa-harness`, `pa-adapters`.
