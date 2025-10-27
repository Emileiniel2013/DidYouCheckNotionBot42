# 🤖 DidYouCheckNotionBot42

A Slack bot that catches you slacking 😏 — literally.  
It reads messages from your Slack channels and replies with snarky comments + Notion links when your question is already answered in the official 42 Heilbronn documentation.

---

## ⚙️ Features
- 🧩 Connects **Slack → Flask → n8n**
- 🧠 Uses **OpenRouter AI** to classify messages into Notion topics
- 💬 Responds only if the message is *actually* Notion-related
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

This repo includes the **complete workflow export** (`/n8n_workflow/DidYouCheckNotion.json`) —  
so you can just **import it directly** into your own n8n instance. 🚀

### To use it:
1. Open your n8n dashboard → click **Import Workflow** → upload `/n8n_workflow/DidYouCheckNotion.json`
2. Update the credentials:
   - **Slack App:** use your own Bot Token + Signing Secret
   - **HTTP Node:** point it to your Flask API (e.g. `https://your-server.com/respond`)
   - **OpenRouter:** add your own API key in the `.env`

The structure of the workflow:
1. **Trigger:** Slack Trigger (new message posted)
2. **HTTP Request:** Sends `{ "message": <text>, "user": <id> }` to Flask
3. **Condition:** If `reply` ≠ `none`
4. **Slack Reply Node:** Posts the reply in the same thread

---

## 📸 Example Outputs

### 💬 Slack Bot Replies
![Bot Screenshot1](https://i.imgur.com/9l483Vp.png)
![Bot Screenshot2](https://i.imgur.com/MTmAbyh.png)
![Bot Screenshot3](https://i.imgur.com/aklBPFV.png)

---

## 🔐 Data & Privacy
This repo doesn’t include proprietary Notion data.  
- To run with your own workspace, add `NOTION_TOKEN` + `JSON_PATH` to the `.env` file  
- For a quick demo, the app works with `DYCN_bot/notion_entries_sample.json` (synthetic)

---

## 🧃 Credits
Built by **Emileiniel2013** & **@silndoj** (42 Heilbronn) 💚  
Powered by:
- [Flask](https://flask.palletsprojects.com/)
- [n8n](https://n8n.io)
- [OpenRouter.ai](https://openrouter.ai)

---

## 🧩 Use & Adapt
This project is open and meant to be **a blueprint**.  
Feel free to take it, remix it, or adapt it to your own Slack workspace, Notion setup, or even a completely different context.  

---

## 💚 Shoutout
Big love to the **42 Heilbronn goats** 🐐 —  
for the support, the chaos, and the endless inspiration that made this project way funnier (and more useful) than it had any right to be.
