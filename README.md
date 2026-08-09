# Use Case: Consumer Chat Content Policy

**YouTube walkthrough:** [Consumer Chat Content Policy — System Design #Shorts](https://youtu.be/M6wJr7kLl6w)

**Design doc:** [docs/DESIGN.md](./docs/DESIGN.md) — architecture, patterns, and why.


**Parent system design:** [06 — Multi-Layer Safety / Moderation](../06-safety-moderation-pipeline.md)

## Users & problem

A mass-market chat app must block high-severity harm, limit over-refusal, and interrupt unsafe streams without destroying UX latency.

## Requirements & SLOs

| Requirement | Target |
|-------------|--------|
| Critical miss rate | Extremely low |
| Added latency | Overlap classifiers with decode |
| UX | Clear refusals; mid-stream stop |
| Consistency | Same policy across web/mobile |

## Design (from parent)

```
Input L0/L1 → policy-aligned model → streaming L2 output checks
  → async L3 review for borderline
  → reason_code + audit
```

Reuse layered plane and fail-closed rules from **06**; integrate with [02 streaming](../02-streaming-token-delivery.md).

## Specializations

| Concern | Consumer choice |
|---------|-----------------|
| Tone | Helpful refusals; avoid scolding |
| Languages | Multilingual classifiers |
| Features | Memory/tools still under same policy |
| Metrics | Track over-refusal as an SLO |

## Failure modes

- Safety outage → fail-closed on critical categories.
- Under-refuse after model upgrade → canary safety gates ([05](../05-model-monitoring-observability.md)).
- Inconsistent app vs API → shared decision plane.




## Design walkthrough (opens on GitHub)

> **Watch on YouTube:** [Consumer Chat Content Policy — System Design #Shorts](https://youtu.be/M6wJr7kLl6w)


![Design overview](docs/video/design-overview.gif)

Full narrated video (download): [docs/video/design-overview.mp4](docs/video/design-overview.mp4)

## Run (self-contained POC)

This folder is a **standalone** project (safe to split into its own GitHub repo).

```bash
cd consumer-chat-content-policy
python -m venv .venv
source .venv/bin/activate   # Windows: .venv\Scripts\activate
pip install -r requirements.txt
PYTHONPATH=. python -m uvicorn app.main:app --reload --port 8000
```

```bash
curl -s http://127.0.0.1:8000/health | jq
```

curl -s -X POST http://127.0.0.1:8000/moderate -H 'Content-Type: application/json' -d '{"text":"hello"}' | jq

---

**Copyright (c) 2026 Debashis Bhattacharjee. All Rights Reserved.**  
Unauthorized copying or redistribution of this material is prohibited.  
GitHub: [Debashis2007](https://github.com/Debashis2007)

