# LinkedIn Content Creator Workflow (n8n)

An automated n8n workflow that generates high-quality LinkedIn posts from topics stored in Google Sheets.

It researches the topic using **Tavily**, writes an engaging post with **Claude 3.5 Sonnet** (via OpenRouter), and saves the finished content back to Google Sheets.

---

## What This Workflow Does

1. Gets a topic from Google Sheets (only rows with status `To Do`)
2. Researches the topic using Tavily Search API
3. Generates a professional LinkedIn post using Claude 3.5 Sonnet
4. Saves the generated content back to Google Sheets and updates the status to `Created`

---

## Tech Stack

| Tool              | Purpose                          |
|-------------------|----------------------------------|
| **n8n**           | Workflow automation              |
| **Google Sheets** | Topic storage + content output   |
| **Tavily API**    | Web research                     |
| **OpenRouter**    | Access to Claude 3.5 Sonnet      |
| **Claude 3.5**    | Content generation               |

---

## Prerequisites

- n8n instance (self-hosted or n8n Cloud)
- Google account
- Tavily API key → https://tavily.com
- OpenRouter API key → https://openrouter.ai

---

## Setup Instructions

### 1. Import the Workflow

1. Open your n8n instance
2. Go to **Workflows → Import from File**
3. Upload the file: `LinkedIn Workflow.json`

### 2. Create Google Sheet

Create a Google Sheet with these columns:

| Topic | Status | Content |
|-------|--------|---------|
| AI in startups | To Do | |
| Remote work trends | To Do | |

- **Topic** → The subject you want a LinkedIn post about  
- **Status** → Use `To Do` for new topics  
- **Content** → Will be filled automatically by the workflow  

After the workflow runs, Status changes to `Created` and the generated post appears in the Content column.

### 3. Connect Credentials in n8n

#### Google Sheets
1. Create a new **Google Sheets OAuth2** credential
2. Connect your Google account
3. Select the credential in both:
   - `Get Topic` node
   - `Send Content` node
4. Update the **Document ID** in both nodes to your own Google Sheet ID

#### Tavily
1. Go to the **Tavily** node
2. Replace the hardcoded API key with your own:
Bearer YOUR_TAVILY_API_KEY
text#### OpenRouter
1. Create a new **OpenRouter API** credential
2. Add your OpenRouter API key
3. Select it in the **OpenRouter Chat Model** node

---

## How the Workflow Works
Manual Trigger
↓
Get Topic (Google Sheets – Status = "To Do")
↓
Tavily (Research the topic)
↓
Content Creator (Claude 3.5 Sonnet via OpenRouter)
↓
Send Content (Update Google Sheets with post + Status = "Created")
text### Prompt Behavior
The AI is instructed to:
- Write a short, inspiring LinkedIn post (max ~700 characters)
- Target entrepreneurs
- Use a motivational and professional tone
- Add 2–4 relevant hashtags
- Include 1–3 tasteful emojis
- Never copy content directly from the research

---

## How to Use

1. Add new topics in your Google Sheet with Status = `To Do`
2. Open the workflow in n8n
3. Click **Test workflow** (or activate it and use a Schedule Trigger later)
4. Check your Google Sheet — the Content column will be filled and Status will change to `Created`

---

## Recommended Improvements

- Replace the Manual Trigger with a Schedule Trigger (e.g. every morning)
- Add a LinkedIn node to auto-post the content
- Add image generation (DALL·E / Flux / Replicate)
- Add a human approval step (Telegram / Slack)
- Store previously used topics to avoid repetition

---

## Important Notes

- The original workflow contains a hardcoded Tavily API key. Replace it with your own key before using.
- Google Sheet Document ID is also hardcoded — update it to your own sheet.
- Make sure your OpenRouter account has access to `anthropic/claude-3.5-sonnet`.

---
