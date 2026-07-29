<p align="center">
  <picture>
    <img alt="Ergo" src="data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 256 256'%3E%3Ccircle cx='128' cy='42' r='22' fill='%23191919'/%3E%3Ccircle cx='60' cy='178' r='22' fill='%23191919'/%3E%3Ccircle cx='196' cy='178' r='22' fill='%23191919'/%3E%3Cline x1='128' y1='64' x2='72' y2='158' stroke='%23191919' stroke-width='2.5' opacity='0.35'/%3E%3Cline x1='128' y1='64' x2='184' y2='158' stroke='%23191919' stroke-width='2.5' opacity='0.35'/%3E%3Cline x1='82' y1='178' x2='174' y2='178' stroke='%23191919' stroke-width='2.5' opacity='0.35'/%3E%3C/svg%3E" width="64" height="64" />
  </picture>
</p>

<h1 align="center">Ergo</h1>

<p align="center">
  <strong>Research that compounds.</strong><br />
  A recursive research agent that decomposes hard questions, investigates every piece with live web search, and synthesizes findings into one verifiable report.
</p>

<p align="center">
  <a href="#how-it-works">How it Works</a> ·
  <a href="#quick-start">Quick Start</a> ·
  <a href="#architecture">Architecture</a> ·
  <a href="#api-keys">API Keys</a>
</p>

---

## How it Works

Ergo is built on the **ROMA** (Recursive Open Meta-Agent) pattern. Instead of asking an LLM one question and getting one shallow answer, Ergo runs a multi-agent recursive pipeline:

```
  Your Question
       │
       ▼
  ┌──────────┐
  │ Atomizer │  Decides: is this simple or complex?
  └────┬─────┘
       │
       ▼
  ┌──────────┐
  │ Planner  │  Breaks complex questions into 2–4 sub-questions
  └────┬─────┘
       │
       ▼
  ┌──────────┐
  │ Executor │  Researches each sub-question with live web search
  └────┬─────┘
       │
       ▼
  ┌────────────┐
  │ Aggregator │  Synthesizes all findings into one coherent report
  └────────────┘
```

The recursion goes up to **2 levels deep** (configurable). Each leaf node calls the Executor agent, which uses **function calling** to search the web via Serper.dev and ground its answer in real-time data — not just model recall.

> **Why ROMA?** Hard problems don't fit in one prompt. ROMA breaks a big task into smaller pieces, hands each to a specialized agent, and combines the results. When a piece is still too big, it splits again — recursively — until every sub-question is simple enough to solve.

---

## Demo

| Landing Page | Research Agent |
|---|---|
| `ergo-landing.html` — Marketing page with animated canvas background | `roma-research-agent.html` — The actual tool |

Open `ergo-landing.html` in your browser. Click **Launch Ergo** or any step (Atomizer / Planner / Executor / Aggregator) to jump into the research agent.

---

## Quick Start

1. **Clone the repo**

   ```bash
   git clone https://github.com/wabrent/ERGO.git
   cd ERGO
   ```

2. **Open the landing page**

   ```bash
   open ergo-landing.html
   ```

3. **Get API keys**

   | Key | Where | Required? |
   |---|---|---|
   | DeepSeek API Key | [platform.deepseek.com](https://platform.deepseek.com) | Yes |
   | Serper.dev API Key | [serper.dev](https://serper.dev) | Optional (enables live web search) |

4. **Configure**

   Open the Settings panel (⚙) in the research agent, paste your API keys, and start researching.

---

## Architecture

```
ergo-landing.html          Marketing page (standalone HTML)
roma-research-agent.html   Research agent (standalone HTML)
```

Both files are **zero-dependency** — no build step, no `npm install`, no framework. Just open in a browser.

### Tech Stack

| Layer | Technology |
|---|---|
| Frontend | Vanilla HTML, CSS, JavaScript |
| Fonts | P22 Mackinac (serif) + Inter (sans) |
| AI Model | DeepSeek Chat (`deepseek-chat`) |
| Web Search | Serper.dev (via function calling) |
| Animation | Canvas 2D API (decomposition tree) |

### ROMA Agent Pipeline

Each research run follows the same sequence:

1. **Atomizer** — Analyzes the question. Returns `{is_atomic: boolean, reason: string}`.
2. **Planner** — If not atomic, splits into `{subtasks: string[]}`.
3. **Executor** — For each atomic sub-question, calls DeepSeek with a `web_search` tool. The model decides when to search; Ergo executes the search via Serper and feeds results back to the model.
4. **Aggregator** — Collects all leaf findings and writes a final synthesized report.

### Function Calling Flow

```
DeepSeek API (with tools)
  → Model returns tool_calls: [web_search]
    → JS executes Serper.dev search
      → Results sent back as role: "tool"
        → Model generates final grounded answer
```

---

## Features

- **Recursive decomposition** — questions get atomized until solvable
- **Live web search** — Executor verifies answers against real-time data via function calling
- **Visual tree** — real-time rendering of the decomposition tree with color-coded status (amber = thinking, teal = done, red = error)
- **Configurable limits** — adjust max recursion depth (1–4) and max leaf nodes (3–30) from the UI
- **Session-only API keys** — keys stored in `sessionStorage`, never in `localStorage`
- **Zero dependencies** — single HTML files, works offline after first load (fonts cached)
- **Dark/light ready** — landing page is light, agent can be restyled

---

## API Keys

Keys are stored in `sessionStorage` only — they are cleared when you close the tab.

| Key | Purpose |
|---|---|
| `ergo_ds_key` | DeepSeek API key |
| `ergo_search_key` | Serper.dev API key |
| `ergo_max_depth` | Recursion depth (default: 2) |
| `ergo_max_leaves` | Max leaf nodes (default: 10) |

Open DevTools → Application → Session Storage to inspect or clear.

---

## License

MIT — use it, fork it, submit a PR.

---

<p align="center">
  Built on <a href="https://github.com/sentient-agi/ROMA">ROMA</a> · Powered by <a href="https://deepseek.com">DeepSeek</a>
</p>
