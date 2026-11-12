# Workflow Test Plan and Test Cases

These are reproducible test specifications for the sanitised portfolio workflows. Unless a result is explicitly marked as executed, it is a planned/manual test and must not be represented as a passing live test.

## Test environment

- Disposable n8n 1.x instance.
- Import one workflow at a time from `workflows/`.
- Keep workflows inactive until configured.
- Use synthetic names, email addresses, postcodes and job details.
- Never use real customer data or production credentials.
- Trigger each webhook using the test URL while the workflow is in test/listening mode.

## 1. Trade Lead Intake and Qualification

Workflow: `trade-lead-intake-qualification-demo.json`

| ID | Input / scenario | Expected result |
|---|---|---|
| LI-01 | Complete valid lead with name, valid email, service and description | accepted=true; status=ready_for_human_review |
| LI-02 | Missing name | accepted=false; name appears in missing_fields |
| LI-03 | Missing service | accepted=false; service appears in missing_fields |
| LI-04 | Invalid email such as `not-an-email` | accepted=false; email_valid=false |
| LI-05 | Email contains mixed case and surrounding spaces | Email is trimmed and lowercased |
| LI-06 | Description contains “urgent leak” | priority=high |
| LI-07 | Ordinary description | priority=normal |
| LI-08 | Postcode supplied in lowercase | Postcode is normalised to uppercase |
| LI-09 | Optional phone omitted | Workflow returns an empty phone string and does not fail |
| LI-10 | Customer submits incomplete data | No message is sent; payload is marked for correction/review |

## 2. Job Brief to Estimate Checklist

Workflow: `trade-job-brief-estimate-checklist-demo.json`

| ID | Input / scenario | Expected result |
|---|---|---|
| ES-01 | Description, postcode, access notes and positive length/height supplied | ready_for_pricing=true; no missing-information questions |
| ES-02 | No measurements | Questions request length and height |
| ES-03 | Length supplied but height absent | Height is identified as missing |
| ES-04 | Zero or negative measurement | Measurement is treated as missing |
| ES-05 | No postcode | Postcode question is returned |
| ES-06 | No access notes | Access question is returned |
| ES-07 | No description or voice-note transcript | Description question is returned |
| ES-08 | Supplied measurements | Each measurement is labelled customer_or_contractor_supplied and unverified |
| ES-09 | Photos supplied | Photo count is reported; no dimensions are inferred from images |
| ES-10 | Any incomplete brief | No final price or quote is produced |

## 3. Completed Job to Marketing Draft

Workflow: `completed-job-to-marketing-draft-demo.json`

| ID | Input / scenario | Expected result |
|---|---|---|
| MC-01 | Business, service and work summary supplied | Draft copy includes supplied factual details |
| MC-02 | Location omitted | No location is invented |
| MC-03 | Customer/property photo URLs supplied without permission | allowed_for_publication=false |
| MC-04 | Explicit media_permission=true | allowed_for_publication=true, but human approval remains required |
| MC-05 | No photos supplied | Workflow still creates text drafts |
| MC-06 | Output generated | approval status is awaiting_human_approval |
| MC-07 | Any input | publish_automatically=false |

## 4. Existing LinkedIn Connection Nudge

Workflow: `linkedin-connection-nudge.json`

The repository README records code-level sample checks for daily message generation, count logging, `/stats`, ignoring other chats and weekly-cap enforcement. Repeat these checks after importing into the target n8n version. Live Telegram webhook behaviour has not yet been verified.

| ID | Scenario | Expected result |
|---|---|---|
| LN-01 | Weekday schedule fires | Daily prompt is built for Europe/London |
| LN-02 | Weekend | No weekday prompt |
| LN-03 | Valid count reply from allowed chat | Weekly and total counts increment |
| LN-04 | Reply from an unapproved chat | Ignored; no state change |
| LN-05 | `/stats` from allowed chat | Weekly and total counts returned |
| LN-06 | Weekly limit reached | Further target is zero and stop guidance is returned |
| LN-07 | New UK week begins | Weekly count resets; total remains |

## Cross-workflow security and reliability checks

- [ ] Exports contain no credentials, tokens, credential IDs, customer data or pinned production data.
- [ ] Webhook endpoints are not activated publicly without authentication/rate limiting appropriate to the deployment.
- [ ] Invalid payloads do not cause unintended side effects.
- [ ] Duplicate webhook delivery does not create duplicate customer actions (idempotency must be added before connecting a CRM or messaging provider).
- [ ] Error handling and alerting are configured before production use.
- [ ] Logs avoid unnecessary personal data.
- [ ] Customer messages, quotes, invoices and social publishing require explicit approval.
- [ ] Import and execution tested on the intended n8n version.
- [ ] Workflow timezone is explicitly configured and checked for daylight-saving transitions.

## Evidence record

Record for each test: date, n8n version, workflow name, test ID, input fixture, expected result, actual result, pass/fail, execution ID (if safe to share), and tester. Do not claim a test passed without an observed result.
