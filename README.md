# Smart Lead Routing & AI Triage (n8n)

An n8n workflow that qualifies inbound leads with an LLM and routes them by intent.

```
Webhook (POST) → Basic LLM Chain (Google Gemini) → If high intent
   ├─ true  → Airtable: create record → Telegram message to sales
   └─ false → Airtable: create record (archive)
```

- **Real-time intake.** A webhook receives the lead as it arrives.
- **AI qualification.** A basic LLM chain classifies intent. You can swap the Gemini model node
  for an Anthropic chat model in n8n without changing the rest of the graph.
- **Priority routing.** High-intent leads are written to Airtable and sent to sales on Telegram.
  Low-intent leads are archived in Airtable.
- **Error handling.** The workflow is meant to run with a separate global error-handler workflow
  that sends a Telegram message on any failure. That handler is not part of this export.

## Setup

1. n8n → Import from file → `workflows/smart-lead-routing-ai-triage.json`.
2. Credentials (shown as `REPLACE_ME`): Google Gemini (PaLM) API, Airtable personal access token,
   Telegram bot.
3. Set your Airtable base and tables (`appXXXXXXXXXXXXXX`, `tblXXXXXXXXXXXXXX`) and your Telegram
   chat ID (`YOUR_CHAT_ID`).
4. Send a test lead: `POST` a JSON body with the lead's fields to the webhook URL.

This is a workflow export, not a measured benchmark: it has no test set or accuracy numbers.
Credential IDs, base and table IDs and chat IDs in the export were replaced with placeholders.
