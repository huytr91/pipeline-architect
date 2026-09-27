# Evidence 001-vietnamese-scanned-report — Vietnamese scanned / annual reports

**Question:** Which extract pipeline is faster and more stable for multi-page Vietnamese report PDFs on a CPU-only machine?

## Workload

4 multi-page VI report/scan PDFs · CPU-only · local

- Samples: **4** PDFs (bytes not published)
- Runs per sample: **3**
- Pipelines: probe / lite / deep (pypdf text-layer)
- Hardware: `Intel64 Family 6 Model 186 Stepping 3, GenuineIntel` · 10 cores · RAM `8-16GB` · GPU `none`

## Prior (recommendation — not evidence)

Prior guess: deeper page budget (deep) should always win on quality; probe is only for smoke checks.

## Measured (this machine, this workload)

| Pipeline | p50 latency | p95 | Peak RAM | Success | Rel. quality* | Mean text_chars | Likely-scan share |
|----------|------------:|----:|---------:|--------:|--------------:|----------------:|------------------:|
| A · probe (max 2 pages) | 1.1 s | 10.3 s | 142 MB | 100% | 0.294 | 341 | 75% |
| B · lite (max 5 pages) | 2.5 s | 1.1 min | 136 MB | 100% | 0.407 | 4465 | 50% |
| C · deep (max 20 pages) | 18.2 s | 40.0 s | 118 MB | 100% | 0.298 | 24696 | 25% |

\*Rel. quality = cross-pipeline agreement on extracted text — **not** CER/accuracy.

## What happened?

- Fastest p50 latency: **A · probe (max 2 pages)** (1.1 s).
- Most extracted text_chars (mean): **C · deep (max 20 pages)** (24696).
- Highest peak RAM: **A · probe (max 2 pages)** (142 MB).
- **Prior ≠ Measured:** deeper/more complex is not automatically the winner on this workload/hardware — and many files look like scans (low text_chars).

## Honesty boundary

- Measured: latency, RAM, success, extracted `text_chars`
- **Not** measured: scan-OCR accuracy, DOCX fidelity, table cell correctness
- Adapter: `pdf-native-parser` via **pypdf** (text layer). Image-only scans → ~0 chars.

## Files

- [`result.json`](./result.json) — raw export
- [`environment.json`](./environment.json)
- [`command.txt`](./command.txt) — reproduce
- [`problem.yaml`](./problem.yaml)

The result is specific to this workload and hardware. It is not a universal ranking.
