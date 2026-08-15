# LinkedIn AI Content Engine

An n8n-powered automation that researches a topic, ghostwrites an original LinkedIn post in a defined voice, generates a matching image prompt, and prepares it for publishing — all with zero manual writing.

Built by [Vaibhav Kate](https://linkedin.com/in/vaibhav-kate) as part of his AI engineering / automation portfolio.

---

## What it does

Give it a topic. It:

1. Searches the web for current, accurate facts, examples, and real-world use cases on that topic
2. Ghostwrites an original LinkedIn post (never copies source sentences) in a defined voice and structure — hook, breakdown, real example, takeaway, CTA
3. Formats the post LinkedIn-native (short lines, mobile-scannable, no markdown symbols)
4. Generates hashtags and an AI image prompt (flat-design / tech-illustration style, no text/logos/real people)
5. Outputs a clean, structured result ready to review and publish

No manual research, no blank-page problem, no generic AI-sounding copy.

---

## Tech stack

| Component        | Role                                                   |
|-------------------|---------------------------------------------------------|
| **n8n**           | Workflow orchestration / automation engine             |
| **LLM API (Claude / GPT)** | Research synthesis + post generation via system prompt |
| **Web Search**    | Pulls current facts, examples, and use cases per topic  |
| **Telegram / Webhook trigger** | Way to submit a topic and receive the draft         |
| **Airtable / Sheets (optional)** | Content calendar / draft log                     |

---

## Architecture (high level)

```
Topic input (Telegram / Form / Webhook)
        │
        ▼
  n8n Trigger Node
        │
        ▼
  Web Search Node  ──► gathers current facts, examples, use cases
        │
        ▼
  LLM Node (system prompt: ghostwriter persona)
        │
        ▼
  Structured Output Parser
        │        └── post_text
        │        └── hashtags
        │        └── image_prompt
        ▼
  Delivery Node (Telegram reply / Airtable row / Notion page)
```

---

## Setup

### Prerequisites
- n8n instance (self-hosted or cloud) — see deployment notes below
- API key for your chosen LLM provider (Claude / OpenAI)
- Web search API access (or n8n's built-in search tool/node)
- (Optional) Airtable base or Notion database for storing drafts
- (Optional) Telegram bot token if using Telegram as the trigger/interface

### Steps
1. Clone this repo
   ```bash
   git clone https://github.com/<your-username>/linkedin-ai-content-engine.git
   ```
2. Import `workflow.json` into your n8n instance (**Workflows → Import from File**)
3. Add credentials in n8n:
   - LLM API key
   - Web search credential
   - Telegram bot token (if used)
   - Airtable/Notion credential (if used)
4. Update the system prompt node with your own voice, background, and posting rules (see `/prompts/ghostwriter-system-prompt.md`)
5. Activate the workflow
6. Send a topic via your chosen trigger and get back a ready-to-post draft

---

## Repo structure

```
linkedin-ai-content-engine/
├── workflow.json                     # n8n workflow export
├── prompts/
│   └── ghostwriter-system-prompt.md  # the ghostwriting system prompt
├── examples/
│   └── sample-outputs.md             # example generated posts
├── docs/
│   └── deployment.md                 # free/low-cost n8n hosting notes
└── README.md
```

---

## Roadmap
- [ ] Auto-publish to LinkedIn via API (currently draft-and-review)
- [ ] Content calendar view in Airtable
- [ ] Multi-topic batch generation
- [ ] Performance tracking (engagement feedback loop into prompt tuning)

---

## Author

**Vaibhav Kate** — AI/ML Engineer & Automation Builder
Building AI-powered n8n systems (RAG pipelines, LLM agents, Telegram/Airtable/Supabase integrations) for real businesses.

[LinkedIn](https://linkedin.com/in/vaibhav-kate)

---

## License

MIT — feel free to fork and adapt for your own content workflows.
