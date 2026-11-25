# Generative UI Demo

> Ask in plain English, and an AI **builds a live, interactive interface for you**, streaming it into the page as it's written.

Generative UI Demo is a chat app where Claude doesn't just answer in text. When you ask for something visual, such as a calculator, a chart, a dashboard, a flowchart or a timer, the model composes a real HTML/SVG widget and you watch it render **token by token** in a side panel. The widgets work: buttons, sliders and inputs function, and they can send data back to the AI for follow-up answers.

**🌐 Live demo: [generative-ui-demo-pi.vercel.app](https://generative-ui-demo-pi.vercel.app)**

[![Open in GitHub Codespaces](https://github.com/codespaces/badge.svg)](https://codespaces.new/KiranChandY/Generative-UI-demo)

---

## ✨ Features

- **Live streaming preview.** The widget appears and grows in real time while the model is still writing it.
- **Tool use as the rendering primitive.** The model calls a `show_widget` tool instead of dumping raw HTML into chat, so prose and UI stream in separate channels.
- **Mid-stream JSON parsing.** Tool arguments are parsed while still incomplete, so the HTML can be rendered before the JSON closes.
- **Flicker-free updates.** [morphdom](https://github.com/patrick-steele-idem/morphdom) diffs the DOM on each update instead of replacing it.
- **Safe script execution.** `<script>` tags run once the widget is complete.
- **Automatic theming.** Widgets only use shared CSS variables, so they always match the app's design system.
- **On-demand design guidelines.** A `load_guidelines` tool pulls in chart, diagram, mockup or interactive rules only when needed, which keeps prompts small.
- **Two-way bridge.** Widgets call `window.sendToAgent(data)` to send user interactions back into the conversation.

## 🧱 Tech Stack

| Layer    | Tech |
|----------|------|
| Backend  | Python, FastAPI, Uvicorn, Server-Sent Events (SSE) |
| AI       | Anthropic Claude API (streaming + tool use) |
| Frontend | Vanilla HTML/CSS/JS, morphdom |

## 📁 Project Structure

```
Generative-UI-demo/
├── server.py              # FastAPI app: /chat SSE endpoint, tool loop, partial-JSON parser
├── tools.py               # Tool schemas: load_guidelines, show_widget
├── system.py              # System prompt with the widget rules and CSS variable contract
├── guidelines/            # Design-system modules loaded on demand
│   ├── core.md
│   ├── chart.md
│   ├── diagram.md
│   ├── interactive.md
│   └── mockup.md
├── public/
│   └── index.html         # Chat UI + live widget panel (served at "/")
├── docs/
│   └── GENERATIVE_UI_EXPLAINED.md   # Deep-dive on how everything works
├── requirements.txt
├── .env.example           # Template for your API key
└── .devcontainer/         # One-click GitHub Codespaces setup
```

---

## 🚀 Getting Started

You need an **Anthropic API key**, which you can create at [console.anthropic.com](https://console.anthropic.com/). The app makes live calls to Claude, so it can't run without one.

### Option 0: Just try it

Open the hosted version at **https://generative-ui-demo-pi.vercel.app**. Nothing to install.

### Option 1: Live preview in your browser (no install)

1. Click the **Open in GitHub Codespaces** badge above, or go to **Code → Codespaces → Create codespace on main** on GitHub.
2. When asked, paste your `ANTHROPIC_API_KEY`. If you're not asked, add it under GitHub **Settings → Codespaces → Secrets**, or run `cp .env.example .env` in the codespace and edit the file.
3. Dependencies install automatically. In the codespace terminal, run:
   ```bash
   python server.py
   ```
4. A **preview tab opens automatically** on port `8000`. If it doesn't, open the **Ports** tab and click the 🌐 globe icon next to port 8000.

### Option 2: Run locally

**Prerequisites:** Python 3.10+ and Git.

```bash
git clone https://github.com/KiranChandY/Generative-UI-demo.git
```

```bash
cd Generative-UI-demo
```

Create a virtual environment (recommended):

```bash
python -m venv .venv
```

```bash
source .venv/bin/activate
```

On Windows, run `.venv\Scripts\activate` instead.

```bash
pip install -r requirements.txt
```

Add your API key:

```bash
cp .env.example .env
```

Then open `.env` and set `ANTHROPIC_API_KEY=...`.

Start the server:

```bash
python server.py
```

You can also start it with `uvicorn server:app --reload`.

Then open **[http://localhost:8000](http://localhost:8000)** in your browser. That's the live preview.

### ⚙️ Configuration

| Variable            | Required | Default           | Description |
|---------------------|----------|-------------------|-------------|
| `ANTHROPIC_API_KEY` | ✅       | –                 | Your Anthropic API key |
| `MODEL`             | ❌       | `claude-haiku-5-5` | Claude model used for generation |
| `PORT`              | ❌       | `8000`            | Port used by `python server.py` |

---

## 💡 Try It Out

Type any of these into the chat:

- `Give me a compound interest calculator`
- `Show me a bar chart of monthly sales`
- `Create a countdown timer`
- `Draw a flowchart of the HTTP request lifecycle`
- `Build a dashboard with key metrics`

The widget streams into the right-hand panel as it's generated. Interact with it, and the results go back to the AI for a follow-up.

## 🔍 How It Works

```
Browser ──POST /chat──▶ FastAPI ──stream──▶ Claude API
   ▲                       │
   │   SSE events:         │  • text deltas → chat bubble
   └── text / widget_delta │  • show_widget JSON deltas → partial parse → live HTML
       widget_final / done │  • load_guidelines → inject design rules, loop again
```

1. The browser sends the chat history to `POST /chat`.
2. The server streams Claude's response. Text goes straight to the chat, while `show_widget` arguments are parsed **while still incomplete** and sent out as `widget_delta` events.
3. The frontend morphs each partial HTML snapshot into the widget panel, then runs scripts on `widget_final`.
4. If Claude calls a tool, the server runs it, adds the result to the conversation, and loops until Claude is done.

For the full walkthrough, see **[docs/GENERATIVE_UI_EXPLAINED.md](docs/GENERATIVE_UI_EXPLAINED.md)**. It covers mid-stream JSON parsing, DOM diffing, script execution, CSS-variable theming, lazy guideline injection and the multi-turn tool loop.

## ☁️ Deployment

The live demo runs on [Vercel](https://vercel.com). Vercel detects the FastAPI app in `server.py` and serves `public/` from its CDN. Every push to `main` redeploys automatically. To deploy your own copy:

1. Import the repo at [vercel.com/new](https://vercel.com/new). No build settings are needed.
2. Under **Settings → Environment Variables**, add `ANTHROPIC_API_KEY`.
3. Redeploy.

## 🛠️ Troubleshooting

- **`AuthenticationError` / no response:** check that `ANTHROPIC_API_KEY` is set in `.env` or in your Codespaces secrets.
- **Model not found:** set `MODEL` in `.env` to a Claude model your account can use.
- **Port already in use:** run `PORT=8080 python server.py` and open `http://localhost:8080`.

## 🙏 Acknowledgements

This project builds on the open-source [generative-ui-demo](https://github.com/sausi-7/generative-ui-demo) by sausi-7.
