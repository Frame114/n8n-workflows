# Workflow Catalogue

A short index for reviewers and maintainers.

| File | Trigger | Purpose | External credentials | State |
|---|---|---|---|---|
| `linkedin-connection-nudge.json` | Schedule + Telegram Trigger | Human-led networking reminder and capped self-reported counter | Telegram bot | Existing sample logic tested; live instance verification pending |
| `trade-lead-intake-qualification-demo.json` | Webhook | Validate and normalise a trade enquiry into a human-review payload | None | Demo export; import/runtime verification pending |
| `trade-job-brief-estimate-checklist-demo.json` | Webhook | Convert a job brief into a missing-information checklist | None | Demo export; import/runtime verification pending |
| `completed-job-to-marketing-draft-demo.json` | Webhook | Create channel-specific marketing drafts with media permission gate | None | Demo export; import/runtime verification pending |

## Common conventions

- All new examples use synthetic data and no live provider credentials.
- Webhook workflows are inactive by default.
- The demos return drafts/review payloads; they do not send messages, calculate authoritative prices, create quotes or publish content.
- Add authentication, rate limiting, idempotency, observability and error workflows before production deployment.
