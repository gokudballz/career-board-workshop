# GHC Workshop Plan — Multi-Agent Framework for Job Search

**Length:** 40 minutes · **Format:** Live code build-along (Python + LLM APIs) · **Audience:** Mixed / broad

**The one takeaway:** Every attendee leaves with a working "career board" — a small multi-agent system that sources roles, scores fit, and drafts tailored pitches — running in their own Google Colab, ready to point at real jobs.

---

## The big idea attendees should internalize

A job search is a pipeline, and you're running five roles at once: sourcing, filtering, tailoring, tracking, follow-up. That's exactly what a **multi-agent system** is good at — one orchestrator coordinating specialized agents that share a common memory. The "career board" is that shared memory made visible.

Three concepts carry the whole talk:

1. **Specialized agents** — each agent has one job and one prompt (do one thing well).
2. **An orchestrator** — plain code that decides who runs when and passes data along.
3. **Shared state (the board)** — a single list of opportunities every agent reads and writes.

If they leave understanding just those three, the workshop worked. The code is how they *feel* it.

---

## The system they'll build

A shared "board" = a list of opportunity records, each moving through columns: **Sourced → Scored → Ready to apply.**

| Agent | Job | Reads | Writes |
|-------|-----|-------|--------|
| **Sourcer** | Pull candidate roles from a job feed given a query | query + sample feed | new opportunities |
| **Fit Scorer** | Score each role 0–100 vs. the user's profile, with a reason | profile + opportunities | score + rationale |
| **Tailor** | Draft a tailored one-line pitch / resume bullet for top roles | profile + top opportunities | pitch text |
| **Orchestrator** | Run the agents in order, update the board, print it | everything | final board |

**Design choice:** build with plain Python + one LLM API (no heavy framework). It avoids dependency/install pain live, and it makes the *pattern* obvious. Name-drop LangGraph / CrewAI / the OpenAI Agents SDK at the end as the "graduate to this next" path.

---

## Minute-by-minute run of show

| Time | Segment | What happens |
|------|---------|--------------|
| **0:00–0:03** | **Hook + finished demo** | Open with the pipeline framing. Run the *completed* board once so they see the destination: roles appear, get scored, top 3 get tailored pitches. "In 35 minutes this is yours." |
| **0:03–0:09** | **Mental model** | One diagram slide: agents + orchestrator + shared board. Map each box to a job-search chore. This is the only "lecture" moment — keep it tight. |
| **0:09–0:13** | **Setup** | Everyone scans the QR → opens the Colab → `Runtime ▸ Run all` on the setup cell (installs + API key). Buddy check: "help the person next to you get a green checkmark." |
| **0:13–0:17** | **Build Round 1 — Sourcer** | Fill in the Sourcer prompt, run, watch candidates populate the board. First dopamine hit. |
| **0:17–0:21** | **Build Round 2 — Fit Scorer** | Add scoring + reasoning; the board sorts itself by fit. |
| **0:21–0:25** | **⚠️ Bias audit — the safety beat** | Re-run the *same* résumé with one variable swapped (name, pronouns, a career gap, a non-elite school). Watch if the score moves. Then add one guardrail line to the prompt and re-run. See below. |
| **0:25–0:29** | **Build Round 3 — Tailor** | Generate a tailored pitch for the top 3 — plus a one-line integrity rule: *tailor emphasis, never fabricate experience.* |
| **0:29–0:31** | **Build Round 4 — Orchestrator** | Wire it together: one `run()` call executes all three agents and prints the full board. "This is the multi-agent part." |
| **0:31–0:35** | **Make it yours** | Swap the sample `profile` string for their own (skills, location, what they want). Re-run. The board changes. *This is the "I built my own career board" moment.* |
| **0:35–0:40** | **Extend + responsible-AI wrap + Q&A** | Plug in real job sources, add agents, frameworks to graduate to, and the safety checklist. Resources QR. Take 2–3 questions. |

**Buffer logic:** the build rounds are the flex zone. If wifi/time slips, Rounds 1–2 + the bias audit + "make it yours" is the minimum viable workshop; Rounds 3–4 are the stretch. **Keep the bias audit even under time pressure — it's the beat people will remember.**

---

## Making a code build-along work for a MIXED audience

This is the core risk — a live Python build with non-coders in the room. Mitigate by design, not by hoping:

- **Pre-scaffold everything.** The Colab ships with imports, the API client, sample data, and the board-printing function already written and tested. Attendees only edit **prompt strings and 2–3 short function bodies.** Nobody starts from a blank cell.
- **Every cell runs on its own.** Each round's cell works even if the previous edit was wrong, because the scaffold provides a fallback/sample. No one gets stranded by a typo.
- **Copy-paste escape hatch.** Each round has a collapsed "✅ Solution" cell. If someone falls behind, they run it and rejoin. No shame, no stalling the room.
- **Two lanes, one notebook.** Core path = edit the prompt and run. "🚀 Stretch" notes in markdown cells for engineers (add retry logic, structured outputs, a 4th agent) so the strong coders don't get bored.
- **Pair people up** during setup so troubleshooting is distributed, not all on you.

---

## ⚠️ The agent-safety beat (0:21–0:25)

The moment your Fit Scorer assigns a number to a human being, you've built an automated hiring filter — the same class of system that has a documented history of encoding bias (famously, a large tech company scrapped an internal résumé-screening tool after it learned to penalize the word "women's"). At GHC, an audience that has lived on the receiving end of that will feel this immediately. Make it experiential, not preachy.

**Run it live (this is the powerful part):**

1. Take one sample résumé the Scorer already rated. Copy it.
2. Change **one** variable only — swap the name (e.g., a gender- or ethnicity-coded name), the pronouns, add a two-year career gap, or downgrade the school from elite to state.
3. Re-run the Scorer on both versions and **compare the scores and the reasoning text side by side.** Sometimes the number moves; more often the *rationale* reveals the bias ("less prestigious background," "gap in employment"). Either way, there's your discussion.
4. **Then fix it in one line.** Add a guardrail to the Scorer prompt — e.g., *"Score only against the listed job requirements. Ignore name, gender, age, school prestige, and employment gaps. If you cannot justify a point against a stated requirement, don't deduct it."* Re-run and see the reasoning change.

That arc — *observe the bias → mitigate it in a prompt → verify the change* — is the whole responsible-AI lesson in 4 minutes, and it's hands-on.

**The talking point:** you didn't remove bias, you made it *visible and auditable*. A human still makes the call. That's the standard to hold your own agents to.

### The five agent-safety risks in a job-search system

Reference these on the closing slide; hit bias + fabrication live, mention the rest.

1. **Biased scoring** — LLMs infer demographics from names, schools, gaps. Mitigation: score against explicit criteria only, require per-point rationale, keep a human in the loop.
2. **Fabrication / integrity** — a Tailor agent that invents experience is writing lies onto your application. Mitigation: hard rule to reframe real experience, never manufacture it.
3. **Over-automation** — auto-applying or spamming recruiters at scale burns your reputation and floods employers. Mitigation: agents *draft and recommend*; the human approves and sends.
4. **Privacy** — you're sending your résumé and personal data to a third-party API. Mitigation: know the provider's data-retention terms, redact sensitive fields, use a workshop/test résumé today.
5. **Transparency** — a bare score is a black box. Mitigation: every agent outputs its reasoning, so decisions are inspectable, not oracular.

**Build these into the notebook, don't just talk about them:** the Scorer prompt ships with the anti-bias guardrail and always returns a `reason` field; the Tailor prompt ships with the no-fabrication rule; a markdown cell names the data-privacy tradeoff before anyone pastes a real résumé.

---

## Prep checklist (before the day)

- [ ] Build + test the Colab notebook end to end on a fresh Google account (simulates an attendee).
- [ ] Provide API access with **zero personal-key friction**: pre-provisioned key via a proxy, a workshop key with a rate cap, or a free-tier model. Decide this early — it's the #1 thing that breaks live coding.
- [ ] Add a **"✅ Solution"** cell under every build round (collapsed).
- [ ] **Pre-build the bias-audit pair:** two near-identical sample résumés differing by one variable, so the demo works even if a live edit fumbles. Test that the score/rationale actually shifts before the day.
- [ ] Add a **fully-finished notebook** as a separate link for the finale/backup.
- [ ] Make a short **URL + QR** for: (1) the Colab, (2) the resources page. Put the QR on your title and closing slides.
- [ ] Slides: title, one architecture diagram, closing resources. That's it — the notebook is the star.
- [ ] Record a **2-minute screen capture of the working demo** as a total-failure fallback (dead wifi = you narrate the recording).
- [ ] Time a full dry run out loud. Live coding always runs long; cut ruthlessly to hit 40.
- [ ] Prepare 3 seed questions in case Q&A is quiet.

## Day-of kit

- Laptop + charger, phone hotspot as wifi backup, HDMI/USB-C adapters.
- Font size way up in Colab; high-contrast theme.
- Sample `profile` and sample job feed pre-loaded so the demo never depends on live scraping.

---

## Closing "graduate to this next" resources (for the last slide)

- **Frameworks:** LangGraph, CrewAI, OpenAI Agents SDK, AutoGen — same pattern, more structure.
- **Real data sources:** job board APIs / RSS, or a scraping step as a new Sourcer.
- **More agents to add:** Interview-prep agent, Networking/outreach agent, Follow-up/tracker agent, Application-status agent.
- **Make the board persistent:** save to a Google Sheet or a simple DB so it's a living tracker, not a one-run demo.

---

## What I can build for you next

- The **complete Colab notebook** (scaffold + solution cells + finished backup version).
- The **slide deck** (title, architecture diagram, closing resources).
- A **one-page attendee handout** with the QR, the 3 concepts, and the extend-it checklist.

Tell me which to start with and I'll build it.
