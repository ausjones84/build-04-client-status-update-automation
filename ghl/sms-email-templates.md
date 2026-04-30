# SMS & Email Templates — Build #4

All message templates used by the Client Status Update Automation. Edit these to match your firm's voice and branding.

---

## Stage-Based SMS Templates

Each template fires when a contact moves into that stage. Variables are auto-populated by n8n.

### New Lead
```
Hi {{firstName}}, thank you for reaching out to us! Your case inquiry has been received and assigned to our intake team. We will contact you within 24 hours. - The Legal Team
```

### Consultation Scheduled
```
Hi {{firstName}}, your consultation has been scheduled! Please check your email for the date, time, and location details. We look forward to meeting with you. - The Legal Team
```

### Documents Requested
```
Hi {{firstName}}, to move your case forward we need a few documents from you. Please check your email for the document checklist. Questions? Reply to this message. - The Legal Team
```

### Under Review
```
Hi {{firstName}}, your case is currently under review by our legal team. We will have an update for you within 3-5 business days. We appreciate your patience. - The Legal Team
```

### Demand Letter Sent
```
Hi {{firstName}}, an important update: a demand letter has been sent to the opposing party. We are awaiting their response. We will notify you immediately when we hear back. - The Legal Team
```

### Negotiation
```
Hi {{firstName}}, your case is in active negotiation. Our team is working hard to reach the best possible outcome for you. We will keep you updated throughout the process. - The Legal Team
```

### Settlement Reached
```
Hi {{firstName}}, great news! A settlement has been reached in your case. Our team will contact you shortly to review the terms and next steps. - The Legal Team
```

### Closed Won
```
Hi {{firstName}}, your case has been successfully resolved! It was our privilege to represent you. Please check your email for final case documents. - The Legal Team
```

### Closed Lost
```
Hi {{firstName}}, after careful review we were unable to proceed with your case at this time. We appreciate you considering us. Please call if you have any questions. - The Legal Team
```

---

## Weekly Check-In SMS Template

Fires after 7 days of no pipeline stage movement.

```
Hi {{firstName}}, just checking in! Your case is currently in the {{stage}} stage. Our team is actively working on it. If you have any questions or concerns, please reply or call us anytime. - The Legal Team
```

---

## Email Templates

### Stage Update Email Subject Lines

| Stage | Subject Line |
|-------|-------------|
| New Lead | Welcome — Your Case Has Been Received |
| Consultation Scheduled | Appointment Confirmed — See Details Inside |
| Documents Requested | Action Required: Documents Needed for Your Case |
| Under Review | Case Update: Under Legal Review |
| Demand Letter Sent | Case Update: Demand Letter Sent |
| Negotiation | Case Update: Active Negotiation Underway |
| Settlement Reached | Important: Settlement Reached in Your Case |
| Closed Won | Your Case Has Been Successfully Resolved |
| Closed Lost | Update Regarding Your Case Inquiry |

### Standard Email Footer

```
This is an automated update from our case management system.

If you have questions, please:
- Reply to this email
- Call us at {{firmPhone}}
- Visit {{firmWebsite}}

Our team is always here to help.

Warm regards,
{{firmName}}
{{firmAddress}}

To unsubscribe from case update emails, reply STOP.
```

---

## Customization Notes

1. Replace all {{variable}} placeholders with your firm's actual GHL custom field tokens
2. Keep SMS messages under 160 characters when possible to avoid multi-part SMS charges
3. Test templates on a staging contact before going live
4. Add your firm name and contact info to every message footer
