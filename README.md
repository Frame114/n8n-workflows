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

The three portfolio demos use Webhook, Code and Respond to Webhook nodes and require no external credentials.

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
