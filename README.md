# 🧭 Build Your Own Career Board

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/gokudballz/career-board-workshop/blob/main/career_board_workshop.ipynb)

**▶️ Run the workshop:** https://colab.research.google.com/github/gokudballz/career-board-workshop/blob/main/career_board_workshop.ipynb

A hands-on GHC workshop: build a small **multi-agent** system that works your job search.
A **Sourcer** finds roles, a **Fit Scorer** ranks them against your profile, and a **Tailor**
drafts a pitch for the best ones — an **Orchestrator** runs them in order and updates a shared
**board**.

## Run it (attendees)

1. Open `career_board_workshop.ipynb` in **Google Colab**
   ([colab.research.google.com](https://colab.research.google.com) → *File ▸ Open notebook ▸ GitHub* → paste this repo URL).
2. Get a **free** Groq API key (no credit card): https://console.groq.com/keys
3. Run the cells top to bottom. Cells marked **✏️ YOUR TURN** are where you write a prompt;
   each has a collapsed **✅ Solution** cell below it.

## What you'll build

| Agent | One job |
|-------|---------|
| Sourcer | Pull plausible roles from a job feed |
| Fit Scorer | Score each role 0–100 vs. your profile, with a reason |
| Tailor | Draft a one-line pitch for the top roles |
| Orchestrator | Run them in order, update the shared board |

Plus a **bias audit** beat: score the same candidate twice with one variable changed, then toggle
a guardrail and compare. Guardrails (anti-bias scoring, no-fabrication tailoring, privacy) are
baked into the notebook.

## Repo contents

- `career_board_workshop.ipynb` — the workshop notebook (the deliverable attendees keep).
- `docs/workshop_plan.md` — facilitator plan: run-of-show, prep checklist, safety beat.

## Notes

- Runs on **open-weight models** (Llama / Qwen / GPT-OSS) served by Groq. Default:
  `llama-3.3-70b-versatile`. Groq can retire model names without notice, so the Preflight cell
  verifies the model and auto-switches to a working fallback if needed.
- **Free tier is ~14,400 requests/day** — a full run of this notebook is ~47 calls, so there's
  plenty of headroom for a workshop. `ask_llm` also retries with backoff on rate limits.
- **Never commit API keys.** The notebook reads the key from a Colab secret or a prompt at runtime.

---
GHC workshop · presenters: <your name> & <co-presenter>
