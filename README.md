# n8n Automation Templates

Reference n8n workflow templates distilled from automation systems I build for real clients at [ShopHelp](https://shophelp.chat) (AI sales chatbot for Shopify/WooCommerce) and [NeverBlast](https://neverblast.com) (AI cold-email outreach).

I can't publish client workflows directly (client data/credentials are private), so these are **rebuilt, anonymized reference versions** of the same patterns — the node structure, branching logic, and error-handling approach are the real thing I ship; the business data is replaced with generic examples.

## Why these three

Most "automation portfolio" repos show a single happy-path flow. These three are chosen because together they cover the three problems that actually come up in production automation work:

| Workflow | Problem it solves | Core n8n pattern |
|---|---|---|
| [`lead-capture-ai-classify`](lead-capture-ai-classify.json) | A webhook fires on every form/chat lead. Most of them are noise (spam, low-intent). You need to triage before a human (or a CRM) ever sees them. | Webhook → AI classification (IF/Switch on model output) → conditional CRM write + conditional Slack/email alert |
| [`cold-email-follow-up-sequencer`](cold-email-follow-up-sequencer.json) | Outreach emails need a 3-step follow-up cadence, but must stop immediately the moment the lead replies — otherwise you look like a bot. | Cron-triggered batch → wait/delay nodes → reply-check guard before every send → sequence state tracked back to the source sheet/DB |
| [`api-retry-with-backoff`](api-retry-with-backoff.json) | Third-party APIs (payment, CRM, email providers) fail intermittently. A naive HTTP Request node just breaks the whole automation on a 429/500. | HTTP Request → error branch → exponential backoff wait → bounded retry loop → dead-letter path after max attempts |

## How to use

1. Open n8n → **Workflows → Import from File**.
2. Import the `.json` file you need.
3. Replace the placeholder credentials (webhook URLs, API keys, sheet/CRM IDs) — every credential field is a clearly named placeholder, nothing is wired to a real account.
4. Each workflow's **Sticky Note** nodes (inside the JSON) explain what each branch is doing, so you don't have to reverse-engineer the logic.

## Stack this reflects

Python / FastAPI for any custom logic node that n8n's built-ins can't express cleanly, REST API integrations, webhook-driven architectures, and LLM calls (classification, generation) wired directly into the automation graph rather than bolted on afterward.

## About me

Shohel Rana — full-stack developer and AI automation engineer. I build and run [ShopHelp.chat](https://shophelp.chat) and [NeverBlast](https://neverblast.com) end-to-end, solo. Contact: mrshohelux@gmail.com.
