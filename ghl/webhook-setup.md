# GHL Webhook Setup — Build #4

This doc covers how to configure GoHighLevel to send pipeline stage change events to your n8n instance.

---

## Overview

GHL sends a POST request to your n8n webhook URL whenever a contact moves to a new pipeline stage. n8n receives this payload and triggers the notification workflow.

---

## Step 1: Get Your n8n Webhook URL

1. Open your n8n instance
2. Open the workflow: **Build #4 - Stage Change Trigger**
3. Click the **GHL Webhook Trigger** node
4. Copy the webhook URL — it will look like:
   `https://your-n8n.domain.com/webhook/ghl-stage-change`
5. Keep this URL — you need it in Step 3

---

## Step 2: Create a GHL Automation Workflow

1. In GHL, navigate to **Automation > Workflows**
2. Click **+ Create Workflow**
3. Name it: `Build #4 - Stage Change Notifier`
4. Click **+ Add Trigger**
5. Select trigger type: **Pipeline Stage Changed**
6. Select your pipeline: `Legal Case Management`
7. Leave stages as "Any Stage" (to catch all transitions)
8. Click Save Trigger

---

## Step 3: Add Webhook Action

1. Click **+ Add Action**
2. Select **Webhook**
3. Set Method: **POST**
4. Paste your n8n webhook URL from Step 1
5. Set Content-Type: `application/json`
6. In the Request Body section, add this JSON payload:

```json
{
  "type": "ContactStageUpdate",
  "contact_id": "{{contact.id}}",
  "pipeline_stage_name": "{{opportunity.stage_name}}",
  "pipeline_id": "{{opportunity.pipeline_id}}",
  "contact_name": "{{contact.name}}",
  "contact_phone": "{{contact.phone}}",
  "contact_email": "{{contact.email}}",
  "location_id": "{{location.id}}",
  "timestamp": "{{now}}"
}
```

7. Click **Save Action**

---

## Step 4: Publish the Workflow

1. Toggle the workflow from **Draft** to **Published**
2. Confirm the activation dialog

---

## Step 5: Test the Integration

1. Create a test contact in GHL
2. Add them to your pipeline at the first stage
3. Manually move them to the next stage
4. Go to n8n — check **Executions** for the workflow
5. You should see a successful execution with the contact data

Expected n8n execution log:
- Node: GHL Webhook Trigger — Status: Success
- Node: Filter Stage Changes — Status: Success (true branch)
- Node: Get Contact Details — Status: Success
- Node: Build Message — Status: Success
- Node: Send SMS via GHL — Status: Success
- Node: Send Email via GHL — Status: Success

---

## Troubleshooting

| Issue | Solution |
|-------|----------|
| Webhook not firing | Check workflow is Published, not Draft |
| n8n shows no executions | Verify webhook URL is correct and n8n is running |
| 401 Unauthorized on GHL API | Check GHL API key in n8n credentials |
| SMS not delivered | Check GHL LC Phone or Twilio is configured and has credits |
| Contact not found | Ensure contact_id is being passed correctly in payload |

---

## GHL API Key Setup in n8n

1. In n8n, go to **Settings > Credentials**
2. Click **+ Add Credential**
3. Search for **GoHighLevel API**
4. Enter your GHL API key (from GHL Settings > Integrations > API Keys)
5. Enter Location ID: `YNe2uLT4ELor6jODZnlR`
6. Save and name it: `GHL - Mr Cees AI`
7. In each GHL-connected node, select this credential
