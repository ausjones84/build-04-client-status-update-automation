# GHL Pipeline Stage Setup — Build #4

This guide walks through configuring GoHighLevel pipelines and stages for the Client Status Update Automation.

---

## Prerequisites

- GHL sub-account with admin access
- Active pipeline (or create a new one specifically for legal case management)
- n8n instance running with workflow-stage-change-trigger.json imported

---

## Step 1: Create or Configure Your Pipeline

1. In GHL, go to **CRM > Pipelines**
2. Click **+ Add Pipeline** or select an existing pipeline
3. Name it: `Legal Case Management`
4. Note the **Pipeline ID** from the URL — you will need this for your .env file

---

## Step 2: Configure Pipeline Stages

Create the following stages in order (customize to match your firm's workflow):

| Stage Name | Description | Triggers SMS/Email |
|------------|-------------|-------------------|
| New Lead | Just entered the system | Yes |
| Consultation Scheduled | Meeting booked | Yes |
| Documents Requested | Waiting on client docs | Yes |
| Under Review | Legal team reviewing | Yes |
| Demand Letter Sent | Letter sent to opposing party | Yes |
| Negotiation | Active negotiation phase | Yes |
| Settlement Reached | Deal agreed upon | Yes |
| Closed Won | Case successfully resolved | Yes |
| Closed Lost | Case did not proceed | Yes |

To add stages:
1. Click **+ Add Stage** in the pipeline editor
2. Name the stage exactly as listed above (must match system-prompt message keys)
3. Assign a color for visual identification
4. Save each stage

---

## Step 3: Enable Pipeline Automation

1. Go to **Automation > Workflows** in GHL
2. Create a new workflow with trigger: **Pipeline Stage Changed**
3. Select your `Legal Case Management` pipeline
4. Set action: **Webhook** — POST to your n8n webhook URL
5. Include payload fields:
   - `contact_id` — {{contact.id}}
   - `type` — ContactStageUpdate
   - `pipeline_stage_name` — {{opportunity.stage_name}}
   - `pipeline_id` — {{opportunity.pipeline_id}}
   - `contact_name` — {{contact.name}}
   - `contact_phone` — {{contact.phone}}
   - `contact_email` — {{contact.email}}

---

## Step 4: Get Your IDs for .env

After setup, collect these values:

```
GHL_PIPELINE_ID=your-pipeline-id-here
GHL_LOCATION_ID=YNe2uLT4ELor6jODZnlR
N8N_WEBHOOK_URL=https://your-n8n-instance.com/webhook/ghl-stage-change
```

Find Pipeline ID:
- Navigate to CRM > Pipelines
- Click on your pipeline
- Copy the ID from the browser URL

---

## Step 5: Test the Pipeline Trigger

1. Create a test contact in GHL
2. Add them to the `Legal Case Management` pipeline at `New Lead` stage
3. Move the contact to `Consultation Scheduled`
4. Check n8n execution log — the webhook should have fired
5. Verify the test contact received an SMS and email

---

## Troubleshooting

- **Webhook not firing:** Check GHL Workflow is published (not draft) and the n8n webhook URL is correct
- **SMS not sending:** Verify GHL has active SMS credits and Twilio/LC Phone is configured
- **Wrong stage name:** Stage names in GHL must exactly match the keys in workflow-stage-change-trigger.json Build Message node
