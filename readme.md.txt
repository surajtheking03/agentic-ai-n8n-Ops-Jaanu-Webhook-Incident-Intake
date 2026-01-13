📘 GitHub README
Ops Jaanu — Webhook-Driven Incident Intake & Ops Notification (n8n)
🔥 Overview

Ops Jaanu is an event-driven Ops automation workflow built using n8n, Jira, and Slack.
It simulates how real-world production incidents are ingested via a webhook, converted into Jira tickets, and routed to Ops teams with severity-aware notifications.

This project focuses on shipping a working, testable automation, not over-engineering prematurely.

🎯 Why This Project Exists

In real Ops environments:

Incidents often arrive from external systems (monitors, APIs, forms)

Severity determines urgency

Teams need immediate visibility, not dashboards no one checks

This workflow demonstrates:

Clean webhook ingestion

Canonical payload design

Ticket creation

Conditional routing based on severity

Human-readable Slack notifications

🧩 Workflow Architecture (Node-by-Node)
1️⃣ Webhook — Incident Intake

Entry point for the workflow

Accepts a JSON payload representing an incident

Example fields:

{
  "incident_id": "INC-2026-001",
  "severity": "high",
  "description": "API latency spike"
}

2️⃣ Canonical Decision Payload

Normalizes incoming data

Acts as the single source of truth

Prevents downstream nodes from breaking if inputs change

Why this matters:
In real systems, payloads evolve. Canonicalization keeps workflows stable.

3️⃣ Create Jira Issue

Automatically creates a Jira ticket

Maps incident metadata into Jira fields

Returns Jira issue ID & API URL

4️⃣ Extract Jira Artifact

Transforms Jira API response into:

Human-friendly ticket key (KAN-XX)

Browser-friendly ticket URL

Severity passthrough for routing

This node exists purely for clarity & reusability.

5️⃣ IF Node — Severity Routing

Core decision logic

Evaluates:

severity == "high"


True Branch

Critical incidents

Immediate Ops visibility required

False Branch

Low / medium severity

Logged for awareness, no escalation

6️⃣ Slack Node — Critical Alert

Triggered when severity = high

Message includes:

Jira ticket key

Direct Jira URL

Explicit severity

Clear Ops acknowledgment

7️⃣ Slack Node — Non-Critical Log

Triggered when severity ≠ high

Message communicates:

Incident logged

No immediate action required

Keeps noise low while preserving traceability

🧪 How to Test the Workflow
Step 1: Trigger via Webhook

Use Hoppscotch / Postman / curl to POST to the webhook URL.

Step 2: Change Severity

Modify only this field:

"severity": "high"


or

"severity": "low"

Step 3: Observe Behavior
Severity	Jira Ticket	Slack Message
high	✅ Created	🚨 Critical alert
low	✅ Created	ℹ️ Non-critical log

This makes testing deterministic and simple.

🔐 Credential Safety Warning

⚠️ IMPORTANT

All sensitive credentials have been removed before publishing:

Jira API credentials

Slack Bot tokens

Webhook IDs & secrets

You must add your own:

Jira credentials

Slack app & bot token

Webhook configuration

Placeholders are intentionally left to keep this repo safe for public use.

🧠 Design Philosophy

Ship working systems

Prefer clarity over cleverness

Build foundations first, sophistication later

Adapt when tools fail (Notion → Slack fallback)

This workflow is production-thinking, not tutorial fluff.

🚀 Future Enhancements (Optional)

Webhook authentication

Severity enums instead of strings

Retry logic & error handling

Multi-channel routing

Audit logs

(Intentionally excluded to keep scope focused.)

❤️ Credits

Built by Suraj
With unhinged support, patience, and love from Agent Jaanu 🤍🤖