# 🎨 Layout Agent — Compra AI Engineer Intern Assignment

A chat-based layout agent that lets users transform design JSON through natural language instructions.

## Live Demo

Chat with the agent and say things like:
- `"Convert this design to 9:16"`
- `"Keep the product large"`
- `"Move the headline to the top"`
- `"Move the offer badge higher"`
- `"Make the headline smaller"`

---

## Setup Instructions

### Option A — Run as Claude Artifact (Recommended, Zero Setup)

1. Open [claude.ai](https://claude.ai)
2. Paste the contents of `src/App.jsx` into the chat and ask Claude to run it as a React artifact
3. The artifact uses Claude's built-in API proxy — **no API key needed**

### Option B — Local Development

#### Prerequisites
- Node.js 18+
- An Anthropic API key

#### Steps

```bash
# 1. Clone the repo
git clone https://github.com/YOUR_USERNAME/layout-agent
cd layout-agent

# 2. Install dependencies
npm install

# 3. Set your API key
cp .env.example .env
# Edit .env and add: VITE_ANTHROPIC_API_KEY=sk-ant-...

# 4. Start the dev server
npm run dev
```

> ⚠️ **Note on API key security**: For production, proxy API calls through a backend. Never expose your Anthropic key in a client-side app.

---

## Project Structure

```
layout-agent/
├── src/
│   ├── App.jsx          # Main React app (chat + wireframe + JSON viewer)
│   └── main.jsx         # Entry point
├── public/
│   └── index.html
├── README.md
├── APPROACH.md          # Design decisions explained
├── package.json
└── .env.example
```

---

## Features

| Feature | Status |
|---|---|
| Chat interface | ✅ |
| LLM integration (Claude claude-sonnet-4-20250514) | ✅ |
| Layout reasoning via system prompt | ✅ |
| JSON transformation | ✅ |
| Wireframe preview | ✅ |
| Follow-up instructions | ✅ |
| Undo/Redo history | ✅ |
| One-click suggested commands | ✅ |
| Syntax-highlighted JSON view | ✅ |
| Copy JSON button | ✅ |

---

## Tech Stack

- **React** (Vite) — UI framework
- **Claude claude-sonnet-4-20250514** — Layout reasoning LLM
- **Vanilla CSS** — No UI library dependencies
