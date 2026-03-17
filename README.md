# 🤖 PM Job Hunt AI Agent

**An automated AI-powered job hunting pipeline that finds entry-level Product Manager roles in India, discovers HR contacts, drafts personalized cold emails, and logs everything to Google Sheets — all on autopilot.**

Built with n8n + Serper + Hunter.io + OpenAI + Google Sheets

---

## 📌 Problem

Job hunting as an aspiring PM is repetitive and time-consuming — searching for roles, finding the right person to contact, writing personalized emails, and tracking it all. This agent automates the entire pipeline so you can focus on interviews, not inbox management.

## 💡 What It Does

| Step | Action |
|------|--------|
| 🔍 **Search** | Searches Google for entry-level PM jobs in India (runs twice daily) |
| 🧹 **Clean** | Extracts structured data — job title, company, URL, domain — from raw results |
| 📧 **Find Contacts** | Looks up HR/recruiter emails for each company using Hunter.io |
| ✍️ **Draft Emails** | Uses OpenAI GPT-4o-mini to write personalized cold emails per role |
| 📊 **Log & Track** | Saves everything to a Google Sheet for daily review and manual sending |

## 🧩 Workflow Architecture

The agent is a **6-node linear pipeline** in n8n:

```
Schedule Trigger → Serper (Google Search) → JavaScript (Data Cleaning) → Hunter.io (Email Finder) → OpenAI (Email Drafting) → Google Sheets (Output)
```

### Node Breakdown

1. **Schedule Trigger** — Fires automatically every 12 hours
2. **HTTP Request (Serper)** — Searches Google for PM job listings in India via Serper API
3. **Code (JavaScript)** — Cleans raw search results, extracts company domains
4. **HTTP Request (Hunter.io)** — Finds HR/recruiter email addresses per company domain
5. **OpenAI (GPT-4o-mini)** — Drafts a warm, personalized cold email for each role
6. **Google Sheets (Append Row)** — Logs date, job title, company, URL, HR email, email draft, and status

## 🛠️ Tools & APIs

| Tool | Purpose | Free Tier |
|------|---------|-----------|
| [n8n Cloud](https://n8n.io) | Workflow automation engine | ✅ Free tier available |
| [Serper.dev](https://serper.dev) | Google Search API | 2,500 searches/month |
| [Hunter.io](https://hunter.io) | Email finder for company domains | 50 lookups/month |
| [OpenAI](https://platform.openai.com) | AI-powered email drafting (GPT-4o-mini) | $100 free credits |
| [Google Sheets](https://sheets.google.com) | Output storage & tracking | Free |

## 📂 Repository Contents

| File | Description |
|------|-------------|
| `PM_Job_Hunt_Agent.json` | n8n workflow JSON — import directly into any n8n instance |
| `README.md` | This file — setup guide and documentation |

## 🚀 How to Set Up

### Prerequisites
- An [n8n Cloud](https://n8n.io) account (or self-hosted n8n instance)
- API keys for: [Serper.dev](https://serper.dev), [Hunter.io](https://hunter.io), [OpenAI](https://platform.openai.com)
- A Google account for Google Sheets

### Steps

1. **Import the workflow** — Download `PM_Job_Hunt_Agent.json` from this repo. In n8n, go to *Workflows → Import from File* and upload it.

2. **Create a Google Sheet** — Name it `PM Job Outreach Tracker` with these headers in Row 1:

   `Date | Job Title | Company | Job URL | HR Email | Email Draft | Status`

3. **Add your API keys** — In each HTTP Request node, replace the placeholder API keys:
   - Serper node → `X-API-KEY` header
   - Hunter node → `api_key` query parameter
   - OpenAI node → connect your OpenAI credential

4. **Connect Google Sheets** — In the final node, authenticate your Google account and select your sheet.

5. **Activate** — Toggle the workflow to *Active* (top-right corner). It now runs automatically every 12 hours.

### Daily Usage

- Open your Google Sheet each morning
- Review new rows added by the agent
- Copy email drafts into Gmail, personalize if needed, and send
- Update the *Status* column to `Sent` or `Not Interested`

## ⚠️ Troubleshooting

| Issue | Fix |
|-------|-----|
| `URL is not defined` | Use manual domain extraction with `.split('/')` in the Code node |
| No output data returned | Remove `.filter()` and use fallback values with `\|\|` |
| Hunter emails empty | Strip subdomains using `.slice(-2).join('.')` |
| OpenAI rate limit error | Add a Wait node (5–10 seconds) before the AI node |
| Sheets showing `output` | Change operation from *Get Row* to *Append Row* |
| Email Draft is `undefined` | Use the path `$json.output[0].content[0].text` |

## 🔮 Future Roadmap

**Phase 2 — Enhancements**
- Add LinkedIn RSS feed as a second job source
- Duplicate checker to avoid emailing the same company twice
- Telegram notifications when new jobs are found
- Gmail Drafts node to save emails directly in Gmail

**Phase 3 — Advanced**
- Apollo.io as a backup email finder
- Company research node for richer context
- Job relevance scoring system
- Application tracking dashboard

## 👩‍💻 Built By

**Kritika Mittal** — Aspiring Product Manager with experience in user research, product discovery, and MVP prototyping.

*Built with curiosity and caffeine ☕ — March 2026*

---

## 📜 License

This project is open source and available for learning and reference. Feel free to fork, modify, and build upon it.
