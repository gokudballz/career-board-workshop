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
2. Get a **free** Gemini API key (no credit card): https://aistudio.google.com/app/apikey
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

- Default model: `gemini-3.6-flash` (free tier). If Google rotates names, change `MODEL` in the
  setup cell (e.g. `gemini-2.5-flash`).
- **Never commit API keys.** The notebook reads the key from a Colab secret or a prompt at runtime.

---
GHC workshop · presenters: <your name> & <co-presenter>
