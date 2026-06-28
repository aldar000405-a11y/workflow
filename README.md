# AI-Powered Lead Info Extractor



An n8n workflow that parses unstructured business messages into structured, validated JSON using Google Gemini AI.



Built as a practical learning project to move beyond tutorials and build real automation that solves a real problem: turning messy text into clean, actionable data.

---

## What It Does

When someone sends a message like:
> *"Hi, I'm Sarah Johnson from Bright Dental. My email is sarahmohummad@gmail.com and my phone number is +971501234567. We need help automating our appointment reminder system and integrating it with our CRM. This is urgent and should be handled as soon as possible. Our budget is 2000 USD."*

This workflow instantly extracts and validates:
| Field | Example Output |

|-------|---------------|

| `company_name` | Bright Dental |

| `contact_name` | Sarah Johnson |

| `email` | sarahmohummad@gmail.com |

| `service_needed` | automating our appointment reminder system and integrating it with our CRM |

| `urgency_level` | urgent |

| `phone` | +971501234567 |

| `budget_mentioned` | 2000 USD |

**Success Response:**

```json

{

  "success": true,

  "company_name": "Bright Dental",

  "contact_name": "Sarah Johnson",

  "email": "sarahmohummad@gmail.com",

  "service_needed": "automating our appointment reminder system and integrating it with our CRM",

  "urgency_level": "urgent",

  "phone": "+971501234567",

  "budget_mentioned": "2000 USD"

}

Error Response (missing fields or invalid email):

JSON

{

  "success": false,

  "error": "company_name is missing | Invalid email format: user@invalid"

}

Workflow Architecture

plain

Manual Trigger → Edit Fields → Code (Cleanup) → Google Gemini → Code (Validation) → If → Respond to Webhook

Node Breakdown

Table

Node	Purpose

Manual Trigger	Quick testing entry point. Swappable with a Webhook node for production.

Edit Fields	Sets the raw message input for processing.

Code: Cleanup	Trims whitespace and capitalizes the first letter of the message for consistency.

Google Gemini	AI extraction engine. Parses the message and returns strict JSON with required fields. Prompt engineered to never invent data and to flag missing fields.

Code: Validation	Strips markdown fences, parses JSON, checks for missing required fields, and validates email format with regex.

If	Routes the flow based on success boolean.

Respond to Webhook (Success)	Returns clean, structured JSON with all extracted fields.

Respond to Webhook (Error)	Returns detailed error message for debugging.

Requirements

n8n — Self-hosted or n8n Cloud

Google Gemini API Key — Set up via Google AI Studio or Google Cloud

Basic familiarity with n8n expressions and JSON

Setup Instructions

1. Import the Workflow

Download workflow.json from this repo.

In n8n, go to Workflows → Import from File.

Select the downloaded JSON.

2. Configure Credentials

Open the "Message a model" (Google Gemini) node.

Under Credentials, add your Google Gemini API key.

Save the credential.

3. Test the Workflow

Click "Execute Workflow".

The workflow will run using the sample message in the Edit Fields node.

Check the output of Respond to Webhook5 for the success path.

4. Switch to Production (Webhook)

When you're ready to go live:

Replace the Manual Trigger node with a Webhook node.

Set the Webhook node's Respond parameter to "Using Respond to Webhook Node".

Connect your external app or form to the webhook URL.

The rest of the flow remains identical.

Project Context

Known Limitations & Roadmap

Current Limitations

Manual Trigger is used for rapid testing. Not suitable for production — swap for a Webhook node.

Email validation uses a basic regex. It catches obvious typos but is not full RFC 5322 compliant.

No persistence — extracted data is only returned in the response, not stored in a database or CRM yet.

Single-item processing — the Respond to Webhook node only handles the first item in the input.

Roadmap

[ ] Replace Manual Trigger with Webhook node for live endpoint

[ ] Add CRM integration (HubSpot / Pipedrive / Airtable)

[ ] Add error logging and notification (Slack / Email)

[ ] Support batch processing for multiple messages

[ ] Add phone number validation

[ ] Build a simple dashboard to view extracted leads

File Structure

plain
.
├── README.md          # This file

├── workflow.json      # Full n8n workflow export

└── assets/

    └── workflow-screenshot.png   # Visual reference of the canvas

Tech Stack

n8n — Workflow automation

Google Gemini — LLM for information extraction

JavaScript — Custom validation and cleanup logic

License

This project is open for learning and remixing. If you use or improve it, a shoutout is appreciated!

Connect

If you have suggestions for optimizing this workflow, I'd love to hear them. Drop a comment or open an issue.

Built with curiosity and too much coffee ☕
