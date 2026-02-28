# 🔍 OpenScouter — AI Talent Scouting Harness

> Your own AI recruiter. Runs locally. No database needed. The scouter way. 🔍

OpenScouter is a personal AI recruitment assistant built on [OpenClaw](https://github.com/openclaw/openclaw).
It helps you **find, evaluate, match, and contact** talent — using the same messaging channels you already use.

**No centralized talent DB.** Instead, your AI agent searches the open web:

- 🔎 Google search for public profiles, portfolios, blog posts
- 💻 GitHub — contribution history, tech stack, project quality
- ✍️ Tech blogs & content — writing quality, expertise depth
- 💼 LinkedIn / 사람인 / 원티드 — via your logged-in browser session
- 📄 Resume parsing — PDF/text → structured candidate profiles

The agent uses **your browser** (Chrome Extension Relay) to access recruitment platforms with your own login. No API keys, no scraping — just an AI assistant that operates your browser like you would.

## How it works

```
You: "React 시니어 3년+ 서울 찾아줘"
     │
     ▼
OpenScouter Agent
     ├─ Googles "senior react developer seoul blog github"
     ├─ Opens LinkedIn in your browser → searches → extracts profiles
     ├─ Checks GitHub profiles → analyzes contributions
     ├─ Reads tech blogs → evaluates expertise
     │
     ├─ Saves candidates locally:
     │   workspace/candidates/김OO-react-senior.md
     │   workspace/candidates/이OO-fullstack.md
     │
     ├─ Scores & ranks against your JD
     └─ Reports back via Discord/Slack/Telegram
```

## Quick Start

```bash
# Install
npm install -g openscouter@latest

# Onboard (sets up channels, browser, workspace)
openscouter onboard --install-daemon

# Start
openscouter gateway --port 18789

# Talk to your scouter
# Via Discord, Slack, Telegram, or web chat:
# "Find me 3 senior backend engineers with Go experience in Seoul"
```

## Features

- **Multi-source talent discovery** — Google, GitHub, blogs, LinkedIn, job platforms
- **Browser-based access** — uses your logged-in sessions, no API keys needed
- **Local-first storage** — all candidate data as markdown files, no external DB
- **AI-powered matching** — JD ↔ candidate scoring with multi-dimensional evaluation
- **Outreach drafting** — generates personalized recruitment messages
- **Pipeline tracking** — candidate status management via local files
- **Multi-channel** — talk to your scouter on Discord, Slack, Telegram, WhatsApp, etc.

## Bundled Skills

| Skill               | Description                                                            |
| ------------------- | ---------------------------------------------------------------------- |
| `talent-search`     | Multi-source candidate discovery via Google dorking, GitHub, blogs     |
| `google-dorking`    | Advanced Boolean search queries for finding people on the public web   |
| `github-profiler`   | Deep GitHub profile analysis — contributions, code quality, tech stack |
| `candidate-eval`    | Profile analysis, JD matching, multi-dimensional scoring               |
| `outreach`          | Personalized recruitment message generation                            |
| `persistent-search` | Continuous search — keeps finding candidates until you say stop        |
| `jd-manager`        | Job description creation and management                                |

## Architecture

OpenScouter is an [OpenClaw](https://github.com/openclaw/openclaw) fork with:

- Recruitment-specialized workspace templates (SOUL.md, AGENTS.md)
- Bundled talent scouting skills
- Recruiter-persona defaults
- Same harness: channels, tools, sessions, browser, cron — all inherited

## Development

```bash
git clone https://github.com/seokmogu/openscouter.git
cd openscouter
pnpm install
pnpm ui:build
pnpm build
pnpm openclaw onboard --install-daemon
pnpm gateway:watch
```

## Credits

Built on [OpenClaw](https://github.com/openclaw/openclaw) — the personal AI assistant harness.

## License

MIT
