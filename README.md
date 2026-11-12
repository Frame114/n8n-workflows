# n8n Workflows & Engineering Portfolio

A public, sanitised collection of practical n8n workflow demonstrations and engineering work samples. Exports are designed for review and import into a disposable n8n instance. They contain no live credentials or customer data.

**Portfolio overview:** [Engineering & Automation Portfolio](portfolio/README.md)  
**Workflow test plan:** [Test cases and verification checklist](tests/WORKFLOW_TESTS.md)  
**Workflow index:** [Workflow catalogue](docs/WORKFLOW_CATALOGUE.md)

## Workflow catalogue

| Workflow | What it demonstrates | Verification status |
|---|---|---|
| [LinkedIn connection nudge](#linkedin-connection-nudge) | Scheduling, Telegram integration, stateful counters, weekly caps and human-led activity | Code-node sample checks documented; live n8n verification pending |
| [Trade lead intake and qualification](workflows/trade-lead-intake-qualification-demo.json) | Webhook intake, validation, normalisation, priority tagging and human review | JSON syntax checked; n8n import/runtime tests pending |
| [Trade job brief to estimate checklist](workflows/trade-job-brief-estimate-checklist-demo.json) | Job capture, missing-information detection and measurement provenance | JSON syntax checked; n8n import/runtime tests pending |
| [Completed job to marketing draft](workflows/completed-job-to-marketing-draft-demo.json) | Job-to-content repurposing, channel variants and media permission gate | JSON syntax checked; n8n import/runtime tests pending |
| [Customer feedback and review eligibility](workflows/customer-feedback-review-demo.json) | Consent-aware review request eligibility and human approval | JSON syntax checked; n8n runtime pending |
| [Appointment request validation](workflows/appointment-request-demo.json) | Validates appointment requests without falsely confirming availability | JSON syntax checked; n8n runtime pending |
| [Invoice payment follow-up triage](workflows/invoice-followup-demo.json) | Invoice ageing and reminder/escalation recommendations | JSON syntax checked; n8n runtime pending |
| [Supplier price list normalisation](workflows/supplier-price-normalise-demo.json) | SKU, unit, price and duplicate validation for supplier imports | JSON syntax checked; n8n runtime pending |
| [Customer support ticket triage](workflows/support-ticket-triage-demo.json) | Categorisation, priority and security escalation routing | JSON syntax checked; n8n runtime pending |
| [Order status customer update](workflows/order-status-update-demo.json) | Approved-status-to-message drafting with a send approval gate | JSON syntax checked; n8n runtime pending |


## Advanced API-integrated workflows

Five advanced ten-node workflows have been added for employers to inspect. They require external provider accounts and API credentials; all exports are inactive by default.

| Workflow | Integrations | What it demonstrates |
|---|---|---|
| [Lead intake](workflows/advanced-lead-intake-hubspot-slack-sheets.json) | HubSpot, Slack, Google Sheets | CRM upsert, validation, alerting and audit logging |
| [AI support triage](workflows/advanced-ai-support-zendesk-slack.json) | OpenAI, Zendesk, Slack | AI classification, schema validation, ticket creation and escalation |
| [Supplier price monitor](workflows/advanced-supplier-price-monitor-sheets-slack.json) | Supplier API, Google Sheets, Slack | Catalogue normalisation, baseline comparison and price alerts |
| [AI social content pipeline](workflows/advanced-ai-social-content-approval-buffer.json) | OpenAI, Google Sheets, Slack, Buffer | Content generation, draft queue and explicit publishing approval |
| [Payment reconciliation](workflows/advanced-stripe-payment-reconciliation-sheets-slack.json) | Stripe, Google Sheets, Slack | Payment retrieval, event logging and exception notification |

See [advanced workflow setup](docs/ADVANCED_API_WORKFLOWS.md) for required credentials, environment variables, API assumptions and production safeguards. See [advanced workflow tests](tests/ADVANCED_API_WORKFLOW_TESTS.md) for test scenarios.

**Portfolio status:** 15 workflows total — ten introductory examples and five advanced API-integrated examples. The five advanced exports each contain exactly ten nodes. JSON and graph structure have been checked; live provider integration tests are pending. Never add API secrets to this public repository.

## Portfolio and private application work

The [portfolio page](portfolio/README.md) explains the engineering skills demonstrated here and provides high-level, non-confidential case studies of private application work, including AgentBridge and HueSchool. **Their source code is not published in this repository.** The case studies are intentionally limited to product scope and engineering challenges; implementation status is labelled honestly.

## Repository layout

```
workflows/       Sanitised n8n workflow exports
tests/           Test scenarios and verification checklist
docs/            Workflow catalogue and operating notes
portfolio/       Employer-facing engineering portfolio
LICENSE
README.md
```

---

## LinkedIn connection nudge

A weekday Telegram prompt with LinkedIn search links, plus a reply-based weekly invite counter. It does not automate LinkedIn actions.

**Status:** both Code nodes are tested against sample inputs (daily message, logging a count, `/stats`, ignoring other chats, the weekly cap). It has not yet been run on a live n8n instance.

### What it does

- Every weekday at 09:00 (Europe/London) it sends a Telegram message with six LinkedIn people-search links and a suggested number of invites for each, 20 a day in total.
- You reply to the bot with how many invites you sent. It adds that to a weekly and an all-time count; `/stats` returns both.
- It enforces a weekly cap of 100. Today's target shrinks as the week fills up, and once the cap is reached the morning message says no invites today.

### Key decisions

- **Human in the loop:** the workflow builds search URLs only. You review results and take any action yourself.
- **Weekly cap:** daily target is limited by the remaining weekly allowance.
- **State:** workflow static data stores the weekly and all-time counters. There is no external database or history.

### Setup

See the existing workflow notes in this README and the exported file `workflows/linkedin-connection-nudge.json`. Create a Telegram API credential in n8n, import the export, select the credential on Telegram nodes and configure your own allowed chat ID. Never commit the bot token or your chat ID.

---

## Demo workflow setup

The nine portfolio demos use Webhook, Code and Respond to Webhook nodes and require no external credentials. Together with the original LinkedIn workflow, the repository now contains ten workflow examples.

1. Import the JSON export into a disposable n8n instance.
2. Keep it inactive and use the Test URL while listening for a test event.
3. Send synthetic JSON payloads matching the scenarios in [WORKFLOW_TESTS.md](tests/WORKFLOW_TESTS.md).
4. Inspect the response payload and execution output.
5. Add authentication, rate limiting, idempotency, error handling and monitoring before any production use.

### Safety boundaries

- Lead workflow validates and prepares a review payload; it does not message the customer.
- Estimate workflow flags missing data and provenance; it does not infer dimensions or calculate a quote.
- Marketing workflow creates drafts; it does not publish. Photo permission and human approval remain required.
- No workflow in this repository should be connected to real customer data without a privacy/security review.

## Verification policy

A valid JSON export is not proof that a workflow imports or executes correctly in n8n. The new demo exports have been checked for valid JSON structure and node/connection presence. Their n8n import and runtime tests remain pending and are listed in the test plan. Do not represent pending checks as passed.

## Licence

MIT. See [LICENSE](LICENSE).
