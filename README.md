# Build #4: Client Status Update Automation

> **Stack:** GHL + n8n + SMS/Email
> **Pricing:** $1,200 setup + $397/month
> **Problem Solved:** Clients repeatedly calling to ask "what's happening with my case?"

---

## Overview

Build #4 is a fully automated client communication system that monitors GHL pipeline stage changes and proactively sends personalized SMS/email updates to clients. When there is no case movement for 7 days, it automatically sends a check-in message reducing inbound status calls by 40-60%.

---

## How It Works

1. **GHL Pipeline Trigger** - n8n webhook fires whenever a contact moves to a new pipeline stage in GHL
2. **Personalized Update Sent** - Client receives SMS + email: "Hi [Name], your case has moved to [Stage]. Here's what that means..."
3. **7-Day No-Movement Check** - n8n scheduler runs daily; if no stage change in 7 days, sends automated check-in SMS
4. **Attorney Notification** - If client replies with urgent keyword, escalates to attorney via SMS/Slack

---

## File Structure

build-04-client-status-update-automation/
- README.md
- n8n/workflow-stage-change-trigger.json
- n8n/workflow-weekly-checkin.json
- ghl/pipeline-stage-setup.md
- ghl/sms-email-templates.md
- ghl/webhook-setup.md
- docs/setup-guide.md
- docs/pricing.md
- .env.example

---

## Pricing

| Package | Price |
|---------|-------|
| Setup Fee (one-time) | $1,200 |
| Monthly Retainer | $397/month |

---

## Key Features

- Stage-change SMS fires within 60 seconds of pipeline update
- Weekly check-in automation if no movement in 7 days
- Personalized messages per pipeline stage
- Attorney escalation on urgent client replies
- Reduces inbound status calls by 40-60%
- Dual delivery: SMS + email per update
