# Evidence 002-financial-table — Financial statements (BCTC / tables)

**Question:** On Vietnamese financial PDFs (often image/table heavy), does a deeper text-layer extract actually yield more usable text_chars — or just more latency/RAM?

## Workload

7 Vietnamese financial-report PDFs · CPU-only · local

- Samples: **7** PDFs (bytes not published)
- Runs per sample: **3**
- Pipelines: probe / lite / deep (pypdf text-layer)
- Hardware: `Intel64 Family 6 Model 186 Stepping 3, GenuineIntel` · 10 cores · RAM `8-16GB` · GPU `none`

## Prior (recommendation — not evidence)

Prior guess: financial PDFs need deep extract + layout; probe should fail quality.

## Measured (this machine, this workload)

| Pipeline | p50 latency | p95 | Peak RAM | Success | Rel. quality* | Mean text_chars | Likely-scan share |
|----------|------------:|----:|---------:|--------:|--------------:|----------------:|------------------:|
| A · probe (max 2 pages) | 157 ms | 1.7 s | 167 MB | 100% | 0.339 | 355 | 71% |
| B · lite (max 5 pages) | 65 ms | 1.9 s | 214 MB | 100% | 0.339 | 355 | 71% |
| C · deep (max 20 pages) | 139 ms | 2.2 s | 215 MB | 100% | 0.323 | 355 | 71% |

\*Rel. quality = cross-pipeline agreement on extracted text — **not** CER/accuracy.

## What happened?

- Fastest p50 latency: **B · lite (max 5 pages)** (65 ms).
- Most extracted text_chars (mean): **A · probe (max 2 pages)** (355).
- Highest peak RAM: **C · deep (max 20 pages)** (215 MB).
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
