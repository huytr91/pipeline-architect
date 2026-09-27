# Evidence

**Don't trust the README. Run the benchmark — or read the raw results below.**

Pipeline Architect does not ask you to believe that pipeline A is better.  
It runs candidates on a workload and shows **measured** observations.

```text
PRIOR (recommendation)
        ↓
Candidate A ─┐
Candidate B ─┼──→ LOCAL BENCHMARK ──→ OBSERVATIONS
Candidate C ─┘                              ↓
                                      EVIDENCE PACKET
```

## Honesty first

| Claimed here | Not claimed |
|--------------|-------------|
| Latency, peak RAM, success, extracted `text_chars` | Ground-truth CER / field F1 / DOCX fidelity |
| Real PDFs on a real CPU-only machine | Cloud GPU leaderboard numbers |
| **pypdf text-layer** extract (ops) | Full scan-OCR (Paddle/Tesseract) — not published yet |

Many Vietnamese financial/report PDFs behave like **image scans**: text-layer extract returns ~0 chars. That is itself evidence — a “deeper extract” pipeline cannot invent text that is not in the layer.

## Published runs

| ID | Workload | Story |
|----|----------|-------|
| [001-vietnamese-scanned-report](./runs/001-vietnamese-scanned-report/) | 4 multi-page VI reports | Deep gets more chars when text exists; probe is fastest; **not the same winner** |
| [002-financial-table](./runs/002-financial-table/) | 7 BCTC / financial PDFs | ~71% likely-scan — probe/lite/deep extract **the same** mean chars → deep does not win |
| [003-mixed-pdf-batch](./runs/003-mixed-pdf-batch/) | 10 mixed files | Lite wins p50 latency; deep wins text_chars; batch spread is huge |

Each run folder contains:

- `README.md` — Prior vs Measured narrative  
- `result.json` — raw metrics (no PDF bytes)  
- `environment.json` — hardware + harness version  
- `command.txt` — how to reproduce with local samples  
- `problem.yaml` — workload fingerprint  

## Reproduce

PDFs stay on the contributor machine (not in git). With your own samples:

```bash
git clone https://github.com/huytr91/pa-harness.git
cd pa-harness
# put PDFs under evidence-cases/<id>/samples/
python cli.py run --problem evidence-cases/001-vietnamese-scanned-report/problem.yaml \
  --pipelines pipelines/vn-pdf-extract-probe-v1.yaml \
             pipelines/vn-pdf-extract-lite-v1.yaml \
             pipelines/vn-pdf-extract-deep-v1.yaml \
  --samples-dir evidence-cases/001-vietnamese-scanned-report/samples \
  --db bench.duckdb --runs 3
```

**Prior ≠ Measured.** Your machine may disagree with ours — that is the point.
