# Design: Consumer Chat Content Policy

**Project:** `consumer-chat-content-policy`  
**Parent system design:** `06-safety-moderation-pipeline.md`

## 1. What this POC demonstrates

Layered input/output safety with reason codes for consumer chat.

## 2. Architecture (POC)

```text
Input L0/L1 → MockLLM → Output L2 → allow|refuse|block
```

## 3. Patterns used (and why)

| Pattern | Why used | Where in code |
|---------|----------|---------------|
| Layered SafetyPlane | Defense in depth; model alone is insufficient. | `check_input` + `check_output`. |
| Reason codes | Product/UX and audit need machine-readable causes. | `reason_code`. |
| Pre-model block | Don't spend GPU on obvious critical hits. | L0 blocklist. |

## 4. Key endpoints

`GET /health`, `POST /moderate`

## 5. Tradeoffs / POC limits

No streaming mid-output interrupt in this JSON endpoint.

## 6. How to run

See the **Run (self-contained POC)** section in [`../README.md`](../README.md).

This folder is self-contained and can be published as its own GitHub repository.

## 7. Design walkthrough video

> **Watch on YouTube:** [Consumer Chat Content Policy — System Design #Shorts](https://youtu.be/M6wJr7kLl6w)
>
> Direct link: **https://youtu.be/M6wJr7kLl6w**

Also available in-repo:
- GIF preview: [`video/design-overview.gif`](./video/design-overview.gif)
- MP4 download: [`video/design-overview.mp4`](./video/design-overview.mp4)
- Narration script: [`video/narration.txt`](./video/narration.txt)

