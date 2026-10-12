# Agentic Workflows: Presenter Run of Show

**75 minutes · two presenters (A and B) · recorded demonstration · CrewAI → OpenAI Agents SDK → LangGraph**

**The format:** you demonstrate, students watch. Most will arrive late, and the recording serves both the face-to-face and online classes. That shapes everything below: the recording is the product, so narrate for someone who was not in the room.

Materials: `agentic-starter-kits/` (three kits), `agentic_workflow_guide.md` (full guide, for running everything later), `pre_class_setup.md` (optional), `agentic_assignment.md` (the turn-in).

---

## Why this order

CrewAI → Agents SDK → LangGraph moves from **most abstract to most explicit**. CrewAI lets you describe a team in plain language and get results fast. The Agents SDK shows the underlying loop (model → tool → model) with almost no scaffolding. LangGraph then hands over the controls and lets you draw that loop yourself. Each framework makes the previous one's hidden machinery visible.

**Install time is not a class-time problem.** Only the two of you run code live, so nobody waits on installs, and Groq rate limits are unlikely to matter because only your two keys are calling the API. (For reference, CrewAI is the heavy kit at about 770 MB; the other two are about 55 MB.)

---

## Tonight (prep checklist)

Because you are the only ones running code, **your demos are the single point of failure.** Rehearse them.

1. **Run all three kits live with a real Groq key.** I tested the flows against a simulated Groq endpoint, which verifies the code, the tool and hand-off loops, and the model names sent. It cannot predict every rule Groq enforces: the first live run found a schema error in the Agents SDK hand-offs, now fixed. Live latency and model behavior are still untested, so rehearse everything.
2. **Rehearse the three live changes** (listed in each segment below). Prepare each one in a scratch copy so you can paste it if typing goes wrong.
3. **Save backup output:** screenshots or saved terminal text of one good run per kit. If Groq or the Wi-Fi fails mid-recording, you can show these.
4. **Use a throwaway demo key** and delete it after class. Never open `.env` on screen; show `.env.example` instead. The check scripts and kits do not print the key, but an open `.env` tab will.
5. **Confirm the model.** Groq retired `llama-3.3-70b-versatile` and `llama-3.1-8b-instant` on 08/16/26. The kits default to `openai/gpt-oss-120b`; make sure it answers on your key.
6. **Recording setup:** test mic and screen capture; enlarge the editor and terminal fonts (VS Code: `Ctrl/Cmd` and `+`); close notifications and unrelated tabs.
7. **Send the optional message** (below) and post the zip.

### Message to students (optional setup, assignment included)

> **Subject:** Tomorrow: agentic workflows demo (follow along only if you like)
>
> Tomorrow we demonstrate three agent frameworks: CrewAI, the OpenAI Agents SDK, and LangGraph. The session is recorded, and you are not expected to code along.
>
> If you want to follow live, `pre_class_setup.md` takes about 10 minutes (start with the CrewAI folder, the slowest). Either way, the full step-by-step guide and the assignment are in the same download, so you can run everything after watching the recording.

---

## Timeline

| Time | Min | Segment | Drives | Other presenter |
| :--- | :--- | :--- | :--- | :--- |
| 0:00 | 5 | **Opening** | A | B: notes chapter timestamps |
| 0:05 | 18 | **CrewAI** | A (keyboard) | B narrates the run, watches chat if online students are live |
| 0:23 | 17 | **OpenAI Agents SDK** | B (keyboard) | A catches questions |
| 0:40 | 17 | **LangGraph** | B (keyboard) | A at the board explaining |
| 0:57 | 8 | **Compare and choose** | A and B | |
| 1:05 | 7 | **The assignment** | B | A |
| 1:12 | 3 | **Buffer and close** | | |

**Every segment follows the same shape,** which also makes the recording easy to navigate:

1. **Orient (20 to 30 seconds):** say the segment name aloud and recap where we are, for late arrivals and for anyone who jumps into the recording here.
2. **Concept on the board (2 to 3 minutes).**
3. **Run it,** narrating what you are doing and, during any wait, what is happening.
4. **Walk the code.**
5. **Live change:** make one deliberate change and run it again, so students see the pattern they will repeat in the assignment.

---

## 0:00 Opening (5 min) · A

**Say:** An LLM answering once is a chatbot. An *agent* decides what to do next, uses tools, and loops until it is done. Today: three frameworks, three mental models. Write them on the board:

- **CrewAI:** a team of specialists
- **Agents SDK:** one agent with tools and hand-offs
- **LangGraph:** a flowchart you draw

**Say for the recording:** "You do not need to follow along. The full guide and the assignment are posted. Today is about seeing how each one thinks." State the ground rule once: every agent step is a model call, and output varies run to run.

---

## 0:05 CrewAI (18 min) · A drives

| Min | Action |
| :--- | :--- |
| 0:05 | **Orient.** Board: Agent (who), Task (what), Crew (how). |
| 0:08 | `cd crewai-kit`, then `uv run python main.py "your topic"`. Say each command aloud and name the folder you are in. |
| 0:08 to ~0:10 | **Narrate the wait.** B explains what is happening: three agents, three tasks, each output feeding the next. Do not go silent. |
| 0:10 | Open `output/explainer.md` and read a few lines. |
| 0:12 | **Walk `main.py`:** LLM, three agents, three tasks, the crew. Ask the room (and the recording): *why does a backstory change the output?* (it becomes system-prompt context). |
| 0:16 | **Live change:** rewrite the Writer's backstory for a very different audience (a 10-year-old, a skeptical CFO). Re-run and compare tone against the first output. |
| 0:21 | Name the limits: no web tool in this kit, so "research" is the model's own knowledge; more agents means more calls, time, and cost. |

---

## 0:23 OpenAI Agents SDK (17 min) · B drives

| Min | Action |
| :--- | :--- |
| 0:23 | **Orient.** One sentence: *an agent is instructions + tools + hand-offs; the Runner loops until a final answer.* |
| 0:25 | `cd agents-sdk-kit`, then `uv run python main.py`. Demo the three prompts from the guide, calling out the `[Agent Name]` on each reply: **Math Tutor**, **Writing Coach**, then **Triage** answering for itself. |
| 0:30 | **Walk the code:** `@function_tool`, the specialists, the `handoffs` list, the `Runner` loop. Point at `HandoffReason`: hand-offs are tools, and Groq rejects a tool with an empty schema, so each carries a `reason` (it also prints why Triage routed where it did). |
| 0:34 | **The teachable gotcha:** show the `OpenAIChatCompletionsModel` wrapper and explain why. A bare `"openai/gpt-oss-120b"` string gets its `openai/` prefix stripped, so Groq would receive the wrong model name. *Lesson: with a non-default provider, verify the model name that actually goes over the wire.* |
| 0:37 | **Live change:** add a third specialist (for example, a Translator with its own `handoff_description`) and add it to Triage's `handoffs` by copying the existing `handoff(...)` line. Ask something that should route to it. |

---

## 0:40 LangGraph (17 min) · B on the keyboard, A at the board

This is the densest idea, so split it: A draws and explains, B types and runs.

| Min | Action |
| :--- | :--- |
| 0:40 | **Orient.** A draws the graph: `START → agent ⇄ tools → END`. Define State, node, edge, conditional edge. |
| 0:43 | B runs `main.py`. Copy the printed Mermaid text into **mermaid.live** and show that the picture matches the board. |
| 0:46 | **Walk the code** in the same order as the board: `State` (with `add_messages`), the `agent` node, `add_edge`, `add_conditional_edges`, `MemorySaver`. |
| 0:50 | Demo: `What is 17 * 23?`, then the two-tool chain (time, then multiply), then `What was my first question?` to show the checkpointer remembering. Point at the `[agent]` and `[tools]` trace lines. |
| 0:54 | **Live change:** add `interrupt_before=["tools"]` to the compile line, run a question, show the pending tool call, then approve and resume. (This snippet is verified; it is in the guide.) |

---

## 0:57 Compare and choose (8 min) · A and B

1. **One sentence per framework** (A, B, A): the mental model and when you would reach for it. Use the table in Part 4 of the guide.
2. **Say the rule of thumb:** prototype in whichever matches how you think about the problem, and move to LangGraph when you need to control exactly what happens when.
3. **Name the two things that matter next:** **tools** (where agents become useful) and **evaluation** (a handful of test prompts, re-run after every change).

---

## 1:05 The assignment (7 min) · B

Walk through `agentic_assignment.md` on screen. Say plainly, for the recording:

- It is **one framework, one change, one short report.**
- The live changes you just made are the model: small, visible, and explained.
- Show the REPORT.md template. Say it is **graded for effort**: a complete, honest attempt earns full credit, and a change that did not work still counts if it is explained. Say the one exception out loud: **an exposed API key is an automatic 0.** Show the two output files they must save, `run1_original` and `run2_changed`; that is how you verify the work.
- Remind them: **no attachments.** Build a **fresh folder** with only their changed files and `REPORT.md`, share the folder, and submit the link, so `.env` (their API key) and `.venv` (hundreds of megabytes) stay out. Tell them to test the link in a private window.
- State the due date and how to submit.

---

## Recording tips

- **Say the segment name aloud** at each start; B writes down the clock time so you can post chapter timestamps.
- **Say commands aloud** and keep the folder name visible. Viewers cannot ask "which folder?"
- **Repeat questions** before answering; the recording may not capture the room.
- **Never show `.env`.** Show `.env.example`.
- **Do not narrate silence away:** during any model call, explain what is happening.
- If online students are live, B watches the chat and feeds questions to A.

## If you fall behind

Cut in this order:

1. Skip the **live change** in one segment (you can mention it instead; saves about 4 minutes).
2. Shorten the **Compare and choose** discussion to the one-sentence round.
3. Trim the CrewAI limits discussion.

Never cut the assignment walkthrough. Students who watch only the recording need it.

## If something breaks on camera

| Symptom | Move |
| :--- | :--- |
| `429` / rate limit on your key | Set `GROQ_MODEL=openai/gpt-oss-20b` in `.env` (offscreen) and re-run; or switch to your saved backup output |
| "model not found" | The model was retired. Check https://console.groq.com/docs/deprecations and change `GROQ_MODEL` |
| Groq or Wi-Fi is down | Walk the code and board diagrams and show your saved outputs; say so plainly on the recording |
| A live change fails | Say what you expected, show the error, and paste your prepared version. Debugging out loud is useful, but cap it at 2 minutes |
| Output looks different from the guide | Expected; say so once, early |
