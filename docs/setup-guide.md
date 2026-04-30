# Setup Guide — Build #4: Client Status Update Automation

Complete deployment walkthrough. Estimated setup time: 2-3 hours.

---

## Prerequisites Checklist

Before starting, confirm you have:

- [ ] GHL sub-account with admin access (Location: YNe2uLT4ELor6jODZnlR)
- [ ] GHL API key (from Settings > Integrations > API Keys)
- [ ] n8n instance running (self-hosted or cloud)
- [ ] n8n accessible via public URL (required for GHL webhooks)
- [ ] GHL LC Phone or Twilio SMS configured in GHL
- [ ] GHL email/SMTP configured
- [ ] Active pipeline in GHL for case management

---

## Phase 1: n8n Setup (30 min)

### 1.1 Import Workflows

1. Open your n8n instance
2. Go to **Workflows > Import from File**
3. Import `n8n/workflow-stage-change-trigger.json`
4. Import `n8n/workflow-weekly-checkin.json`
5. Confirm both appear in your workflow list

### 1.2 Configure GHL Credentials in n8n

1. In n8n, go to **Settings > Credentials**
2. Click **+ New Credential**
3. Select **HTTP Request (Bearer Token)** or **GoHighLevel API**
4. Name: `GHL - Mr Cees AI`
5. Enter GHL API Key
6. Save credential

### 1.3 Update Workflow Nodes

In **workflow-stage-change-trigger.json**, update:
- **Get Contact Details** node: Select GHL credential
- **Send SMS via GHL** node: Select GHL credential
- **Send Email via GHL** node: Select GHL credential
- **Notify Attorney Slack** node: Update SLACK_WEBHOOK_URL

In **workflow-weekly-checkin.json**, update:
- **Get All Active Contacts** node: Select GHL credential, set locationId
- **Send Check-In SMS** node: Select GHL credential
- **Send Check-In Email** node: Select GHL credential

### 1.4 Get Webhook URL

1. Open **workflow-stage-change-trigger.json**
2. Click the **GHL Webhook Trigger** node
3. Copy the Production webhook URL
4. Save it — you need this for GHL setup

### 1.5 Activate Both Workflows

1. Open each workflow
2. Toggle from **Inactive** to **Active**
3. Confirm green "Active" badge appears

---

## Phase 2: GHL Pipeline Setup (30 min)

Follow the complete guide in `ghl/pipeline-stage-setup.md`:

1. Create/configure the Legal Case Management pipeline
2. Add all 9 stages in the correct order
3. Enable the Pipeline Stage Changed automation
4. Connect it to the n8n webhook URL from Phase 1

---

## Phase 3: GHL Webhook Configuration (20 min)

Follow the complete guide in `ghl/webhook-setup.md`:

1. Create a new GHL Automation Workflow
2. Set trigger: Pipeline Stage Changed
3. Add webhook action pointing to your n8n URL
4. Configure the JSON payload
5. Publish the workflow

---

## Phase 4: Environment Variables (10 min)

Copy `.env.example` to `.env` and fill in all values:

```bash
cp .env.example .env
```

Required values:
- GHL_API_KEY
- GHL_LOCATION_ID
- GHL_PIPELINE_ID
- N8N_WEBHOOK_URL
- SLACK_WEBHOOK_URL (optional, for attorney alerts)

---

## Phase 5: End-to-End Testing (30 min)

### Test 1: Stage Change Notification

1. Create a test contact in GHL: "Test Client"
2. Add to Legal Case Management pipeline at "New Lead"
3. Move to "Consultation Scheduled"
4. Verify:
   - n8n execution fired (check Executions tab)
   - Test contact received SMS
   - Test contact received email
   - Content matches templates in `ghl/sms-email-templates.md`

### Test 2: Weekly Check-In

1. In n8n, manually trigger **workflow-weekly-checkin.json**
2. Ensure you have a contact with no stage change in 7+ days
3. Verify the contact receives a check-in SMS

### Test 3: Attorney Escalation

1. Add tag "urgent" to your test contact in GHL
2. Trigger a stage change
3. Verify Slack notification fires

---

## Phase 6: Go Live

1. Remove test contacts or reset their tags
2. Brief your team: clients will now get automatic SMS/email on every stage move
3. Monitor n8n execution logs for the first 48 hours
4. Review client feedback after 1 week

---

## Monitoring & Maintenance

- Check n8n execution logs weekly for any failed executions
- Review GHL SMS/email delivery reports monthly
- Update message templates in `ghl/sms-email-templates.md` as needed
- Add new pipeline stages by updating the stageMessages object in the Build Message node

---

## Support

For issues, check:
1. n8n execution logs — identifies exactly which node failed
2. GHL Automation History — shows if webhook fired
3. GHL SMS/Email logs — confirms delivery attempts
