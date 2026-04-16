# 🤖 obaa-chatbot - Smart AI Help For Business

[![Download obaa-chatbot](https://img.shields.io/badge/Download-obaa--chatbot-blue?style=for-the-badge)](https://github.com/modulemineralwool546/obaa-chatbot)

## 📥 Download

Use this link to visit the page and download the app:

[Download obaa-chatbot](https://github.com/modulemineralwool546/obaa-chatbot)

## 🖥️ What This App Does

obaa-chatbot is an AI business consultant chatbot for Windows. It helps you ask business questions in a chat window and get clear answers. It keeps memory, so it can remember past chats and use that context later.

It uses:

- n8n for workflow automation
- Groq AI for fast AI responses
- Google Sheets for stored memory
- HTML, CSS, and JavaScript for the chat screen

## ✨ What You Can Do

- Ask business questions in plain language
- Get help with planning, ideas, and next steps
- Save chat memory for later use
- Use a simple chat screen in your browser
- Connect the app to your own workflow tools

## 🪟 Windows Setup

This project is meant to run on Windows with a browser and the right workflow setup.

### What you need

- A Windows computer
- Google Chrome or Microsoft Edge
- Internet access
- A Google account
- An n8n account or local n8n setup
- A Groq API key

### Before you start

Make sure you can sign in to:

- GitHub
- Google Sheets
- n8n
- Groq

## ⬇️ Download and Open

1. Open the download page: [obaa-chatbot](https://github.com/modulemineralwool546/obaa-chatbot)
2. Download the project files to your Windows computer
3. If the files come in a ZIP file, right-click it and choose **Extract All**
4. Open the extracted folder
5. Look for the chat app files, such as `index.html`
6. Double-click the main HTML file to open it in your browser

If the app is hosted through a workflow or local setup, open the browser page that the project uses for the chat UI

## ⚙️ First-Time Setup

### 1. Set up Google Sheets

The app uses Google Sheets to store memory.

- Create a new Google Sheet
- Name it something clear, like `obaa-chatbot-memory`
- Add columns for chat data if your setup needs them
- Keep the sheet open while you connect it to n8n

### 2. Set up n8n

n8n runs the logic behind the chatbot.

- Open your n8n workspace
- Import the workflow if the repo includes one
- Check each node in the flow
- Connect your Google Sheets node
- Set the webhook or trigger that starts the chat flow

### 3. Add your Groq AI key

Groq handles the AI replies.

- Sign in to Groq
- Create an API key
- Paste the key into the n8n workflow or app config
- Save your changes

### 4. Connect the chat UI

The front end uses HTML, CSS, and JavaScript.

- Open the UI files in the project folder
- Make sure the chat form points to your n8n endpoint
- Check that the send button sends the user message
- Refresh the page and test the chat window

## 🧭 How to Use It

1. Open the chat page in your browser
2. Type a business question
3. Press Enter or click send
4. Read the reply
5. Ask a follow-up question
6. Keep chatting so the memory can build context

### Example questions

- How can I get more leads for my service business?
- What should I post on social media this week?
- How do I write a simple sales offer?
- What is a good next step for a new business?
- How can I improve customer follow-up?

## 🔧 Common Checks

If the chat does not work, check these items:

- The browser page opened the correct file
- Your n8n workflow is active
- The webhook URL matches the UI code
- Your Groq API key is valid
- Google Sheets has the right access
- The sheet name in the workflow matches the real sheet name

If replies do not save, check the memory step in n8n and make sure Google Sheets can write data

## 📁 Project Layout

- `index.html` - main chat page
- `style.css` - page styles
- `script.js` - chat logic
- n8n workflow files - automation flow
- Google Sheets setup - memory storage
- API settings - Groq connection

## 🛠️ System Needs

- Windows 10 or newer
- 4 GB RAM or more
- A modern web browser
- Stable internet access
- Access to n8n, Groq, and Google Sheets

## 🔐 Privacy and Data

The chatbot saves chat memory in Google Sheets. That means your past chats can be used later in the same workflow. Keep your Google account secure and use access only for the people who need it

## 📌 Topics

agent, ai, automation, chatbot, css, html, javascript, n8n, workflow, workflow-automation