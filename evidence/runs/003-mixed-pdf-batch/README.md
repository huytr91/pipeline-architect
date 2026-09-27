# Evidence 003-mixed-pdf-batch — Mixed PDF batch (regulation + financial + scan)

**Question:** Which pipeline stays stable across a mixed batch — success rate, latency spread, peak RAM — when file types differ?

## Workload

10 mixed VI PDFs · CPU-only · local · batch-style

- Samples: **10** PDFs (bytes not published)
- Runs per sample: **3**
- Pipelines: probe / lite / deep (pypdf text-layer)
- Hardware: `Intel64 Family 6 Model 186 Stepping 3, GenuineIntel` · 10 cores · RAM `8-16GB` · GPU `none`

## Prior (recommendation — not evidence)

Prior guess: one pipeline ranks best on every file type in a mixed batch.

## Measured (this machine, this workload)

| Pipeline | p50 latency | p95 | Peak RAM | Success | Rel. quality* | Mean text_chars | Likely-scan share |
|----------|------------:|----:|---------:|--------:|--------------:|----------------:|------------------:|
| A · probe (max 2 pages) | 939 ms | 21.4 s | 174 MB | 100% | 0.353 | 976 | 60% |
| B · lite (max 5 pages) | 639 ms | 27.8 s | 191 MB | 100% | 0.351 | 3856 | 50% |
| C · deep (max 20 pages) | 751 ms | 37.4 s | 191 MB | 100% | 0.296 | 14541 | 50% |

\*Rel. quality = cross-pipeline agreement on extracted text — **not** CER/accuracy.

## What happened?

- Fastest p50 latency: **B · lite (max 5 pages)** (639 ms).
- Most extracted text_chars (mean): **C · deep (max 20 pages)** (14541).
- Highest peak RAM: **B · lite (max 5 pages)** (191 MB).
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
