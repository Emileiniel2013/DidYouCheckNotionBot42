# DidYouCheckNotionBot42
A Slack bot that catches you slacking 😏 — literally.  
It reads messages from your Slack channels and replies with snarky comments + Notion links when your question is already answered in the official 42 Heilbronn documentation.

---

## ⚙️ Features
- 🧩 Connects Slack → Flask → n8n
- 🧠 Uses OpenRouter AI to classify messages into Notion topics
- 💬 Responds only if the message is actually Notion-related
- 😎 Replies with a “quirky” roast-style tone (because shame is the best teacher)
- 🔗 Automatically includes the right Notion link for context

---
## 🧠 How It Works

The logic is simple, but spicy:

1. A user posts a message in a Slack channel.  
2. **n8n** triggers the workflow and sends the message + user info to the Flask API (`/respond` endpoint).  
3. The **Flask backend**:
   - Scans your local `notion_data.json` (contains topic, keywords, summary, and reply)
   - Scores how related the message is to each Notion entry
   - Uses **OpenRouter.ai** to confirm the best topic match (or ignore if irrelevant)
4. If a match is found → sends the prewritten witty reply and Notion link back to n8n  
5. n8n posts the response in the same Slack thread 💬  

---

## 🔄 n8n Workflow Setup

This repo includes only the Python part — you’ll need to create your own **n8n workflow** for Slack integration.  
Here’s the structure you should replicate:

1. **Trigger:** Slack Trigger (message posted)  
2. **HTTP Request:** Sends `{ "message": <text>, "user": <id> }` to your Flask API  
3. **Condition:** If `reply` ≠ `none`  
4. **Slack Reply Node:** Sends the `reply` text back to the same thread

👉 You’ll need:
- Your own **Slack App** (with Bot Token + Signing Secret)
- A **publicly accessible Flask endpoint** (for the n8n HTTP Request node)
- Your **OpenRouter API key** in `.env`

---

# Example outputs
![Bot Screenshot1](https://i.imgur.com/9l483Vp.png)
![Bot Screenshot2](https://i.imgur.com/MTmAbyh.png)
![Bot Screenshot3](https://i.imgur.com/aklBPFV.png)

---
## Data & Privacy
This repo doesn’t include proprietary Notion data.
- To run with your own workspace, add NOTION_TOKEN + JSON_PATH to the `.env` file
- For a quick demo, the app works with `DYCN_bot/notion_entries_sample.json` (synthetic).
