# Obaa — AI Business Consultant Chatbot

A persistent AI business consultant chatbot with full conversation memory, user profile personalisation and daily session limiting. Built with n8n, Groq AI and Google Sheets.

---

## Overview

Obaa is an intelligent chatbot designed for Ghanaian businesses. It remembers every conversation, personalises responses based on user profiles and enforces daily usage limits. The entire backend runs on n8n cloud with Google Sheets as the memory layer — no database required.

---

## Features

- **Persistent Memory** — remembers full conversation history across messages using Google Sheets
- **User Profiles** — personalises every response based on stored business context
- **Daily Session Limiting** — enforces configurable daily usage limits per user
- **Professional UI** — dark theme with maroon, gold and black colour palette
- **Real-time Typing Indicator** — animated dots while AI generates response
- **Suggestion Chips** — quick-start prompts on the welcome screen
- **Responsive Design** — works on desktop and mobile browsers
- **Zero Database** — Google Sheets handles all persistence

---

## Tech Stack

| Layer | Technology |
|---|---|
| Frontend | HTML, CSS, JavaScript |
| Backend | n8n cloud |
| AI Model | Groq API — llama-3.3-70b-versatile |
| Memory | Google Sheets |
| Fonts | Cormorant Garamond, DM Sans |

---

## Architecture

```
Chat Interface (obaa-chatbot.html)
          ↓ POST /webhook/chat
n8n Workflow
          ↓
Check Daily Session Limit
          ↓
Fetch User Profile + Conversation History
          ↓
Build Context (messages array with memory)
          ↓
Groq AI — Generate Response
          ↓
Save Messages to Google Sheets
          ↓
Return Response to Chat Interface
```

---

## Setup Guide

### Prerequisites

- n8n cloud account (n8n.io)
- Groq API key (console.groq.com — free tier available)
- Google account
- A web browser

---

### Step 1 — Google Sheets Setup

Create a new Google Sheet named **Chatbot Memory** with two tabs:

**Tab 1 — Conversations:**

| Session ID | User ID | Role | Message | Timestamp | Message Number |
|---|---|---|---|---|---|

**Tab 2 — User Profiles:**

| User ID | Name | Business Name | Industry | Pain Points | Automation Level | Budget Range | Last Active | Total Sessions | Notes | Daily Sessions Used | Last Session Date |
|---|---|---|---|---|---|---|---|---|---|---|---|

Add your first user to User Profiles:
```
USER001 | Your Name | Your Business | Your Industry | Your pain point | beginner | medium | today | 1 | Test user | 0 | (leave empty)
```

---

### Step 2 — n8n Workflow Setup

Import the workflow JSON into your n8n instance:

1. Open n8n cloud dashboard
2. Click **+ New Workflow**
3. Click **⋯** menu → **Import from file**
4. Upload `obaa-workflow.json`
5. Connect your Google Sheets credential
6. Add your Groq API key to the HTTP Request node headers
7. Update the Sheet ID in all Google Sheets nodes
8. Toggle workflow to **Active**

**Webhook paths:**
```
Test:  /webhook-test/chat
Live:  /webhook/chat
```

---

### Step 3 — Configure the Chat Interface

Open `obaa-chatbot.html` and update these values:

```javascript
// Line ~620 in the script section
const N8N_WEBHOOK_URL = 'https://YOUR-N8N-URL.app.n8n.cloud/webhook/chat';
const USER_ID = 'USER001';  // match your User Profiles sheet
```

---

### Step 4 — Deploy

**Option A — Netlify (recommended, free):**
1. Go to netlify.com
2. Drag and drop `obaa-chatbot.html`
3. Rename to `index.html` if prompted
4. Share the generated URL

**Option B — GitHub Pages (free):**
1. Create a new repository
2. Upload `obaa-chatbot.html` renamed to `index.html`
3. Go to Settings → Pages → Deploy from branch
4. Access at `yourusername.github.io/repo-name`

**Option C — Local testing:**
Simply open `obaa-chatbot.html` in any browser.

---

## Configuration

### Change Daily Session Limit

Open the **Check Session Limit** node in n8n and update:

```javascript
const DAILY_SESSION_LIMIT = 3; // change to any number
```

### Change AI Model

In the **Generate Response** HTTP Request node update:

```json
{
  "model": "llama-3.3-70b-versatile"
}
```

Available Groq models:
- `llama-3.3-70b-versatile` — best quality (recommended)
- `llama-3.1-8b-instant` — fastest, lowest cost
- `mixtral-8x7b-32768` — large context window

### Change Conversation Memory Window

In the **Build Context** node update:

```javascript
const recentHistory = sessionHistory.slice(-10); // change -10 to desired number
```

### Customise User Avatar Initial

In `obaa-chatbot.html` find:

```javascript
const avatarHTML = role === 'user'
  ? `<div class="avatar user">T</div>`  // change T to your initial
```

---

## Workflow Nodes

| Node | Purpose |
|---|---|
| Webhook | Receives chat messages from UI |
| Fetch User Profile | Loads user data from Sheets |
| Check Session Limit | Enforces daily usage limit |
| Session Limit Check | Routes blocked vs allowed users |
| Fetch Conversation History | Loads past messages from Sheets |
| Build Context | Assembles AI messages array with memory |
| Generate Response | Calls Groq API with full context |
| Parse Response | Extracts and structures AI output |
| Save User Message | Logs user message to Sheets |
| Save AI Response | Logs AI response to Sheets |
| Update Session Count | Increments daily session counter |
| Respond to Webhook | Returns response to chat interface |

---

## Memory System

Conversation memory is stored in Google Sheets using a role-based structure:

```
Session ID   | User ID  | Role      | Message              | Timestamp
SESSION-001  | USER001  | user      | I need help with...  | 2026-03-20T...
SESSION-001  | USER001  | assistant | Hi Kofi! For your... | 2026-03-20T...
SESSION-001  | USER001  | user      | What would it cost?  | 2026-03-20T...
SESSION-001  | USER001  | assistant | For a restaurant...  | 2026-03-20T...
```

This structure mirrors the OpenAI messages array format exactly — making it trivial to rebuild conversation context for each new message.

---

## Daily Session Limiting

```
User opens chat     → new session created
Message sent        → session count checked
Sessions < limit    → conversation continues
Sessions >= limit   → friendly error returned
                      chat input disabled
Midnight            → count resets automatically
Next day            → full sessions available again
```

Session data is stored in the User Profiles sheet and updated after each new session.

---

## Error Handling

- **safeParseJSON** — all AI responses wrapped in try/catch with fallback values
- **Always Output Data** — conversation history node continues even when empty
- **Limit Reached Response** — graceful message when daily limit exceeded
- **Network Errors** — UI displays friendly reconnection message
- **System Error Handler** — separate n8n workflow notifies via email on any crash

---

## Backup and Recovery

Export your n8n workflow regularly:

1. Open workflow in n8n
2. Click **⋯** → **Download**
3. Save the `.json` file to Google Drive
4. Label with date: `obaa-workflow-2026-03-20.json`

To restore: Import the JSON into any n8n instance and reconnect credentials.

---

## Business Value

```
Without Obaa:
Staff answers business inquiries manually
→ 5-10 minutes per inquiry
→ Only available during business hours
→ Inconsistent responses
→ No memory of previous conversations

With Obaa:
→ Instant responses 24/7
→ Remembers every conversation
→ Personalised to each user's business
→ Consistent professional quality
→ Scales to unlimited users
```

---

## Project Context

Obaa is part of a larger AI automation portfolio built during an intensive automation development programme. Other projects in the portfolio include:

- AI Business Intelligence Dashboard
- Social Media Content Automation System
- Multi-Channel Customer Onboarding System
- AI Product Catalogue and Order System
- AI-Powered Hiring Assistant
- AI Content Pipeline

---

## Author

**Patrina Ofori-Amanfo**
AI Engineer and Automation Expert
Based in Accra, Ghana

---

## License

This project is for portfolio and educational purposes.
