# Marcus — NPC AI Agent

A cinematic NPC chat powered by Claude. Features a pixel portrait, animated rain, city skyline, and a secure backend that hides your API key.

## What's in the box

```
marcus-npc/
├── index.html                    ← The entire frontend (one file)
├── README.md
└── .github/
    └── workflows/
        └── npc-api.yml           ← Secure backend (GitHub Actions)
```

---

## Option A — Quickest: Use with your own API key (no backend)

If you just want to run it locally or don't mind users entering their own key:

1. Open `index.html` in a browser
2. Add a visible API key input back to the HTML (see the previous version)

---

## Option B — Recommended: Secure backend via GitHub Pages + Actions

This keeps your API key secret on the server. Players never see it.

### 1. Push to GitHub

```bash
git init
git add .
git commit -m "marcus npc agent"
# Create a new PUBLIC repo on github.com, then:
git remote add origin https://github.com/YOUR_USERNAME/marcus-npc.git
git push -u origin main
```

### 2. Add your API key as a secret

Go to your repo → **Settings → Secrets and variables → Actions → New repository secret**

| Name | Value |
|------|-------|
| `ANTHROPIC_API_KEY` | `sk-ant-your-key-here` |

### 3. Enable GitHub Pages

Go to **Settings → Pages** → Source: **Deploy from branch** → Branch: `main` → Folder: `/root` → Save

Your site will be live at: `https://YOUR_USERNAME.github.io/marcus-npc`

### 4. Set up the backend proxy

The `npc-api.yml` workflow acts as a serverless backend. It:
- Receives chat messages via `repository_dispatch`
- Calls the Anthropic API with your secret key
- Returns the response

To trigger it from your frontend, update the `BACKEND_URL` in `index.html`:

```js
// Replace this line in index.html:
const BACKEND_URL = './api/chat';

// With a call to GitHub's repository_dispatch API:
// (See full proxy example below)
```

### Full proxy setup (api/chat endpoint)

For a production setup, deploy a tiny proxy (e.g. on Cloudflare Workers or Vercel) that:
1. Receives POST requests from your frontend
2. Triggers the GitHub Actions workflow via `repository_dispatch`
3. Waits for the result and returns it

Or for a simpler approach, use **Cloudflare Workers** free tier as a thin proxy that injects the API key — see `proxy-example/` folder.

---

## Customise Marcus

Edit the top of `index.html` to change your NPC:

```js
// Change name, role, personality:
const SYSTEM = `You are [NAME], a [ROLE] in [SETTING].
Personality: [DESCRIBE PERSONALITY].
...`;

// Change inventory:
let inventory = {
  "Your Item": { qty: 3, price: 100 },
};

// Change lore topics in runTool():
const L = {
  topic_one: "Your lore here.",
  topic_two: "More world building.",
};
```

---

## Tech stack

| Layer | Technology | Cost |
|-------|-----------|------|
| Frontend | Vanilla HTML/CSS/JS | Free |
| Hosting | GitHub Pages | Free |
| Backend | GitHub Actions | Free (2000 min/month) |
| AI | Anthropic Claude claude-sonnet-4-6 | ~$0.003 per conversation |

---

## Get an API key

Sign up at [console.anthropic.com](https://console.anthropic.com)
