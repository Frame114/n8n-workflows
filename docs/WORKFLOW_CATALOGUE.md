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


| `customer-feedback-review-demo.json` | Webhook | Consent-aware review request eligibility | None | Demo export; import/runtime verification pending |
| `appointment-request-demo.json` | Webhook | Validate appointment requests without claiming a slot is booked | None | Demo export; import/runtime verification pending |
| `invoice-followup-demo.json` | Webhook | Invoice ageing and payment follow-up triage | None | Demo export; import/runtime verification pending |
| `supplier-price-normalise-demo.json` | Webhook | Validate supplier price rows and duplicate SKUs | None | Demo export; import/runtime verification pending |
| `support-ticket-triage-demo.json` | Webhook | Categorise and prioritise support requests | None | Demo export; import/runtime verification pending |
| `order-status-update-demo.json` | Webhook | Draft customer updates from approved job status | None | Demo export; import/runtime verification pending |

## Portfolio count

Ten workflow examples are now catalogued: one LinkedIn/Telegram workflow and nine webhook-based demonstrations. All six additions are inactive, use synthetic inputs, require no external credentials and do not send customer messages or update live business systems.
