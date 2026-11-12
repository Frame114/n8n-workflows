# Engineering & Automation Portfolio

**Frame114 — Full-stack development, AI-enabled workflows and business automation**

This public repository is a deliberately sanitised portfolio of practical automation work. It is intended to help employers and collaborators review workflow design, JavaScript data handling, validation, human-in-the-loop controls, documentation and test thinking without requiring access to private commercial application source code.

## What this portfolio demonstrates

- Designing n8n workflows with clear trigger, transformation and response boundaries.
- Building practical lead-intake and qualification logic for service businesses.
- Turning incomplete job briefs into structured estimate checklists without inventing measurements.
- Converting completed-job information into channel-specific marketing drafts.
- Using explicit approval gates before customer-facing actions.
- Documenting setup, assumptions, limitations, security and test cases.
- Thinking about privacy, tenant separation, data provenance, retries and operational reliability.

## Selected work samples

| Sample | Demonstrates | Current evidence |
|---|---|---|
| [LinkedIn connection nudge](../README.md#linkedin-connection-nudge) | Scheduling, Telegram integration, stateful counters, rate caps and safe human-led activity | Code-node logic tested against sample inputs; not yet live-instance verified |
| [Trade lead intake and qualification](../workflows/trade-lead-intake-qualification-demo.json) | Webhook intake, input validation, normalisation, priority tagging and safe review state | Export committed; import/runtime tests pending |
| [Job brief to estimate checklist](../workflows/trade-job-brief-estimate-checklist-demo.json) | Structured job capture, missing-information checks and measurement provenance | Export committed; import/runtime tests pending |
| [Completed job to marketing draft](../workflows/completed-job-to-marketing-draft-demo.json) | Content repurposing, channel variants and image-permission approval gate | Export committed; import/runtime tests pending |

## Private application case studies

The following are private products. Their source code, secrets, customer data, internal implementation details and unreleased assets are intentionally not published here. The summaries below describe product and engineering problem-solving at a high level only.

### AgentBridge — AI-enabled operating platform for UK trades

A product concept and private application focused on connecting marketing, lead capture, job scoping, estimating, supplier pricing, quotes, job tracking and invoicing.

**Engineering problems addressed:** multi-tenant SaaS architecture, role-based access, tenant data isolation, AI-assisted structured extraction, supplier price provenance, deterministic financial calculations, approval workflows, immutable quote/invoice records and external integration adapters.

**My contribution:** product direction, requirements, architecture planning, workflow design and application development. Implementation status should be described as in progress unless a feature has been independently verified.

**Public evidence:** this repository contains small, sanitised workflow demonstrations inspired by generic trade-business processes. It does not contain AgentBridge application source.

### HueSchool — private learning application

A separate private application and codebase. It is kept private to protect the product implementation and development work.

**Public evidence:** no HueSchool source code or private assets are included in this repository. Add only a non-confidential product summary, screenshots specifically cleared for publication, and verified feature claims before using this case study externally.

## Engineering standards shown here

- Never commit API keys, credentials, credential IDs, webhook secrets, customer records or private URLs.
- Use synthetic test data only.
- Keep customer-facing messages, quotes, estimates and publishing behind explicit approval.
- Mark AI-generated values as draft or estimated and preserve source/provenance.
- Do not treat image-only estimates as verified measurements.
- Keep money calculations deterministic and outside LLM-generated text.
- Document what has actually been tested; distinguish code-level checks from live deployment evidence.

## How to review

1. Start with the workflow catalogue in the repository root README.
2. Open each JSON export and its setup notes.
3. Review [workflow test cases](../tests/WORKFLOW_TESTS.md).
4. Import a demo into a disposable n8n instance and use synthetic payloads only.

## Contact

For employment discussions, please use the contact details on my GitHub profile. This repository is a technical work sample, not a claim that every workflow is production deployed.
