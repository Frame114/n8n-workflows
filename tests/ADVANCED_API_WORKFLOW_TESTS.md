# Advanced API Workflow Test Plan

All tests below are planned until an observed n8n execution is recorded. Use sandbox/test accounts and synthetic records only.

## A. HubSpot + Slack + Sheets lead intake

| ID | Test | Expected |
|---|---|---|
| AH-01 | Valid lead payload | Validation passes and HubSpot API receives mapped contact properties |
| AH-02 | Missing name/service | False branch returns HTTP 400; no provider calls |
| AH-03 | Invalid email | Validation fails |
| AH-04 | HubSpot token invalid | Execution fails visibly; configure error alerting and retry policy |
| AH-05 | Slack webhook invalid | Execution reports provider failure |
| AH-06 | Google Sheets scope missing | Append fails; no claim of successful audit |
| AH-07 | Duplicate webhook delivery | Demonstrates duplicate risk; add idempotency before production |

## B. OpenAI + Zendesk + Slack support triage

| ID | Test | Expected |
|---|---|---|
| AS-01 | Normal technical support request | Validated classification and Zendesk ticket creation |
| AS-02 | Prompt-injection text in ticket | Treated as untrusted content; output schema still validated |
| AS-03 | Security incident keywords/classification | Critical security category and Slack escalation |
| AS-04 | Malformed OpenAI JSON | Safe fallback category; inspect and consider routing to manual review |
| AS-05 | Invalid Zendesk credentials | Visible failure; no false ticket-created response |
| AS-06 | Empty message or invalid email | Rejected before API calls |

## C. Supplier API + Sheets + Slack price monitor

| ID | Test | Expected |
|---|---|---|
| AP-01 | Valid supplier product array | Rows normalised with SKU, unit, price and timestamp |
| AP-02 | Missing SKU or invalid/negative price | Row rejected by validation branch |
| AP-03 | Price unchanged from baseline | No change alert |
| AP-04 | Price increased/decreased | Slack change notification and history append |
| AP-05 | Supplier API unavailable | Failure is observable; no stale value is presented as current |
| AP-06 | Duplicate SKU or different pack size | Flag for manual product-equivalence review before estimates change |

## D. OpenAI + Sheets + Slack + Buffer social pipeline

| ID | Test | Expected |
|---|---|---|
| AC-01 | Job not completed | Rejected before AI/API calls |
| AC-02 | Media permission absent | Rejected before AI/API calls |
| AC-03 | Valid approved job summary | AI creates channel drafts and Sheets stores draft |
| AC-04 | Slack approval notification | Reviewer receives draft reference |
| AC-05 | Autopublish disabled | Buffer node must not execute |
| AC-06 | Approval token missing/wrong | Buffer node must not execute |
| AC-07 | Explicit valid approval token and enabled flag in sandbox | Buffer API call may execute; verify provider profile/scheduling schema |
| AC-08 | AI invents unsupported claims | Reviewer rejects; add evaluation and claim checks before production |

## E. Stripe + Sheets + Slack reconciliation

| ID | Test | Expected |
|---|---|---|
| AR-01 | payment_intent.succeeded test event | PaymentIntent retrieved and event row appended |
| AR-02 | payment_intent.payment_failed | Event logged and Slack exception alert generated |
| AR-03 | Unsupported event type | Ignored response; no Stripe retrieval |
| AR-04 | Invalid Stripe API key | Execution fails visibly |
| AR-05 | Duplicate event ID | Duplicate risk demonstrated; implement event-ID idempotency |
| AR-06 | Forged webhook body | Must be rejected after Stripe signature verification is implemented |
| AR-07 | Live mode event | Do not enable until signature verification, replay protection and finance review pass |

## Shared API and security tests

- [ ] Provider secrets stored outside workflow JSON.
- [ ] Least-privilege API scopes configured.
- [ ] API rate-limit and 429 handling tested.
- [ ] Retry policy does not duplicate creates/appends.
- [ ] Timeouts and provider outages are observable.
- [ ] Personal data is minimised in Slack alerts and execution logs.
- [ ] Webhook authentication/signature validation is implemented.
- [ ] Error workflow and operator alerting are tested.
- [ ] Workflow timezone and schedule behaviour are verified.
- [ ] Each provider sandbox response is checked against current API docs.
- [ ] No production activation before approval and evidence record.
