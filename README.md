# WhatsApp GLPI Chatbot

WhatsApp chatbot that lets users open and follow [GLPI](https://glpi-project.org/) helpdesk tickets directly from WhatsApp. It guides the user through a short conversation, classifies the request and creates the ticket through the GLPI REST API.

## Features

- WhatsApp Business **Cloud API** integration (webhook + interactive buttons and lists)
- Guided conversation: request type (incident or request), category, title, description, urgency
- Ticket creation, follow-ups, assignment and tracking through the **GLPI REST API**
- Per-user session management, with a `reset` command to start over
- **AI-assisted classification**: automatic type and category detection, sentiment analysis to help prioritise, complexity estimation, spell correction (TensorFlow.js models, optional OpenAI API)

## Architecture

```
index.js          Express server and WhatsApp webhook
services/         GLPI API client, ticket tracking, WhatsApp messaging, AI analysis
models/           Classification models and vocabularies
```

## Requirements

- Node.js 14+
- A WhatsApp Business account with Cloud API access
- A GLPI instance with the REST API enabled
- A public HTTPS URL for the webhook (ngrok or similar during development)
- An OpenAI API key (optional, for AI analysis)

## Setup

```bash
npm install
```

Create a `.env` file:

```env
# WhatsApp Cloud API
WHATSAPP_TOKEN=your_whatsapp_token
WHATSAPP_PHONE_NUMBER_ID=your_phone_number_id
WHATSAPP_BUSINESS_ACCOUNT_ID=your_business_account_id

# GLPI REST API
GLPI_API_URL=http://your-glpi-server/glpi/apirest.php
GLPI_APP_TOKEN=your_glpi_app_token
GLPI_USER_TOKEN=your_glpi_user_token

# Webhook verification
VERIFY_TOKEN=your_verify_token

PORT=3000
OPENAI_API_KEY=your_openai_api_key
```

Then start the server:

```bash
node index.js
```

## WhatsApp configuration

1. Create an app on [Meta for Developers](https://developers.facebook.com/) and enable WhatsApp Business.
2. Point the webhook to `https://your-domain/webhook` and use your `VERIFY_TOKEN`.
3. Subscribe to the `messages` event.

## GLPI configuration

1. Enable the REST API in GLPI.
2. Create an App token and a User token.
3. Make sure the API user is allowed to create and update tickets.

## License

MIT
