# Advanced API-Integrated Workflow Pack

Five advanced n8n examples, each containing exactly ten nodes. These are portfolio/reference workflows, not turnkey production deployments. All are inactive by default. Configure credentials and test in a disposable n8n instance before activation.

## 1. Lead Intake — HubSpot + Slack + Google Sheets

File: `workflows/advanced-lead-intake-hubspot-slack-sheets.json`

**Flow:** Webhook → validate/normalise → IF → HubSpot contact API → build CRM result → Slack alert → build audit row → Google Sheets append → success response / validation response.

**Connections required**
- HubSpot private app token with minimum CRM contacts write scopes.
- Slack incoming webhook URL.
- Google OAuth access token with access to the selected spreadsheet.
- Google Sheet ID and a `Leads` tab with columns matching the audit row.

**Environment variables:** `HUBSPOT_PRIVATE_APP_TOKEN`, `SLACK_LEAD_WEBHOOK_URL`, `GOOGLE_ACCESS_TOKEN`, `GOOGLE_SHEET_ID`.

**Important:** The example uses the HubSpot create-contact endpoint. Add duplicate/contact lookup and idempotency before production use; do not assume retries are safe.

## 2. AI Support Triage — OpenAI + Zendesk + Slack

File: `workflows/advanced-ai-support-zendesk-slack.json`

**Flow:** Webhook → normalise → IF → OpenAI classification → validate structured result → Zendesk ticket create → prepare escalation → Slack alert → success response / invalid response.

**Connections required**
- OpenAI API key and an enabled model.
- Zendesk subdomain and API token/basic-auth value with ticket create permission.
- Slack incoming webhook URL.

**Environment variables:** `OPENAI_API_KEY`, `OPENAI_MODEL`, `ZENDESK_SUBDOMAIN`, `ZENDESK_BASIC_AUTH_BASE64`, `SLACK_SUPPORT_WEBHOOK_URL`.

**Security:** Treat ticket content as untrusted. The prompt explicitly instructs the model not to follow embedded instructions. Validate output before routing. Add Zendesk requester lookup, rate limits, redaction, retry/idempotency and a human security incident runbook before production.

## 3. Supplier Price Monitor — Supplier API + Google Sheets + Slack

File: `workflows/advanced-supplier-price-monitor-sheets-slack.json`

**Flow:** Daily schedule → supplier catalogue API → normalise rows → IF → read Google Sheets baseline → compare prices → IF → Slack alert → append price snapshot → summary.

**Connections required**
- A supplier catalogue API endpoint and authorised API token.
- Google Sheets OAuth access and spreadsheet ID.
- Slack incoming webhook URL.

**Environment variables:** `SUPPLIER_API_URL`, `SUPPLIER_API_TOKEN`, `GOOGLE_ACCESS_TOKEN`, `GOOGLE_SHEET_ID`, `SLACK_SUPPLIER_WEBHOOK_URL`.

**Data assumptions:** Supplier endpoint returns a `products` array with SKU/product_code, name/description, unit and trade_price/price. Baseline tab `SupplierBaseline` should have SKU in column A and price in column C. Snapshot tab `PriceHistory` should be prepared.

**Important:** Use only supplier feeds/APIs you are authorised to access. Validate product specification, pack size, VAT basis, delivery, currency and effective date. A price movement is not proof that products are equivalent. Review the output before updating estimates.

## 4. AI Social Content — OpenAI + Google Sheets + Slack + Buffer

File: `workflows/advanced-ai-social-content-approval-buffer.json`

**Flow:** Webhook → check job/photo permission → IF → OpenAI draft generation → validate/parse → save draft in Sheets → Slack approval notification → explicit approval-token IF → Buffer create update → response.

**Connections required**
- OpenAI API key/model.
- Google Sheets OAuth access and spreadsheet ID.
- Slack incoming webhook URL.
- Buffer API endpoint/token and profile IDs if publishing is explicitly enabled.

**Environment variables:** `OPENAI_API_KEY`, `OPENAI_MODEL`, `GOOGLE_ACCESS_TOKEN`, `GOOGLE_SHEET_ID`, `SLACK_CONTENT_APPROVAL_WEBHOOK_URL`, `ALLOW_DEMO_AUTOPUBLISH`, `CONTENT_APPROVAL_TOKEN`, `BUFFER_API_URL`, `BUFFER_ACCESS_TOKEN`, `BUFFER_PROFILE_IDS`, `BUFFER_SCHEDULED_AT`.

**Approval safety:** Buffer publishing is blocked unless `ALLOW_DEMO_AUTOPUBLISH=true` and the inbound approval token exactly matches `CONTENT_APPROVAL_TOKEN`. Leave autopublish disabled while testing. Confirm the Buffer API endpoint, profile IDs, API access and scheduling schema against the account's current API documentation before enabling.

## 5. Payment Reconciliation — Stripe + Google Sheets + Slack

File: `workflows/advanced-stripe-payment-reconciliation-sheets-slack.json`

**Flow:** Stripe event webhook → extract event → IF supported PaymentIntent event → retrieve PaymentIntent from Stripe → normalise → append ledger event to Sheets → prepare exception alert → Slack notification → success response / ignored event response.

**Connections required**
- Stripe secret key with read access to PaymentIntents.
- Google Sheets OAuth access and spreadsheet ID.
- Slack incoming webhook URL.
- Stripe webhook endpoint configured to send `payment_intent.succeeded` and `payment_intent.payment_failed`.

**Environment variables:** `STRIPE_SECRET_KEY`, `GOOGLE_ACCESS_TOKEN`, `GOOGLE_SHEET_ID`, `SLACK_FINANCE_WEBHOOK_URL`.

**Critical production requirement:** This demo does not verify Stripe webhook signatures. Do not expose it to live Stripe events until signature verification is implemented using the raw request body and Stripe signing secret, plus replay protection and idempotency keyed by event ID. The example is not a financial ledger or accounting system.

---

## Shared setup and operating rules

1. Import into a disposable n8n instance matching the node versions.
2. Configure credentials/environment variables in the deployment secret store; never put API keys in workflow JSON.
3. Create the required Slack incoming webhooks, Google Sheets tabs and provider API credentials.
4. Replace placeholder supplier and Buffer endpoints with endpoints approved for your account.
5. Test with synthetic data and provider sandbox/test accounts.
6. Keep each workflow inactive until its tests and security review pass.
7. Add retries with backoff, idempotency, rate limits, error workflows, alerting, retention and observability before production.
8. Apply least-privilege OAuth scopes and rotate tokens. Do not share credentials with employers or commit them to GitHub.

## Verification status

The five workflow exports are committed and each has exactly ten nodes. Their JSON syntax and basic graph structure are checked. They have not been imported/executed against live provider accounts; API permissions, endpoint availability, payload compatibility, n8n version compatibility and provider-side approval remain to be verified.
