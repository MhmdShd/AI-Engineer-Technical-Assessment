# ArcVault Intake & Triage Pipeline

## Demo

https://www.loom.com/share/7f8395afc34d43439812c3ad5adacba7

---

## What this is

An automated support triage pipeline that ingests incoming customer messages via webhook, classifies and enriches them using GPT-4o, routes them to the correct team, and flags anything uncertain for human review. All output lands in Google Sheets.

Built with n8n (self-hosted), GPT-4o, and Google Sheets. Total time from spec to working implementation: ~1.5 hours.

---

## Deliverables

| File | Description |
|------|-------------|
| `ArcVault_Architecture_Writeup.docx` | System design, stack choices, routing and escalation logic, and what I would change |
| `ArcVault_Prompt_Documentation.docx` | The prompts used at each LLM step with design rationale |
| Google Sheet (link below) | Structured output for all 5 test messages |
| Loom (link above) | Full walkthrough of the workflow and results |

---

## Test results

All 5 samples processed correctly and consistently across runs:

| # | Category | Priority | Queue | Escalated |
|---|----------|----------|-------|-----------|
| 1 | Bug Report | High | Engineering | No |
| 2 | Feature Request | Low | Product | No |
| 3 | Billing Issue | Medium | Billing | No ($260 < $500) |
| 4 | Technical Question | Low | IT/Security | No |
| 5 | Incident/Outage | High | Human Review | Yes |

---

## Output sheet

[Google Sheets — Triage Output](#) ← replace with your sheet link

---

## How to run

1. Import the n8n workflow JSON into your n8n instance
2. Set up a Header Auth credential named `Kimi API` with your OpenAI key (or point it at your preferred provider)
3. Set up a Google Sheets OAuth2 credential
4. Activate the workflow
5. POST a message to the webhook:

```bash
curl -X POST https://your-n8n-instance/webhook/triage \
  -H "Content-Type: application/json" \
  -d '{"source": "Email", "raw_message": "Your message here"}'
```
