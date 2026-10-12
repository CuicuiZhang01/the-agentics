# Agentic Workflows: Read-Aloud Walkthrough

**Before you start (the terminal):** open the `agentic-starter-kits` folder in VS Code and open a terminal (**Terminal → New Terminal**). Make the fonts large. Do not open `.env` at any point. Every command below is typed from the `agentic-starter-kits` folder, and each segment ends by returning there with `cd ..`.

---

## Introduction

"Today is about three tools for building AI agents. You do not need to code along, because this session is recorded, and the full guide and the assignment are posted.

A chatbot answers once. An **agent** is different: it decides what to do next, it can use tools, such as a calculator or a search, and it keeps going until it has an answer. Think of a research assistant who does not just answer your question but looks things up, checks the arithmetic, and comes back with a result.

We are showing three frameworks, which are three different ways of organizing that assistant:
- **CrewAI** treats it as a *team* of specialists, like a research group with an analyst, a writer and an editor.
- **The OpenAI Agents SDK** treats it as *one assistant* who has tools and can pass you to a specialist.
- **LangGraph** treats it as a *flowchart that you draw*, so you control exactly what happens and when.

One honest warning before we start: every step an agent takes is a call to a language model, so the output is different every time you run it. You will see that today."

**IDEA:**
"As we go, keep one question in mind: where in *your* research is there a multi-step job that you do by hand? Screening papers, cleaning a messy file, running the same analysis on many datasets, drafting a methods summary. Those are the places agents might help."

---

## CrewAI: a team of specialists

"Let's talk about CrewAI. The idea: you describe a team, give each member a job, and CrewAI passes the work from one to the next."

The CrewAI team is Analyst → Writer → Editor. Each member is an Agent (who). Each Agent has a Task (what they do). The Crew (how they work together) passes the work from one to the next.

```
cd crewai-kit
uv run python main.py "retrieval-augmented generation"
```

"Right now three agents are working one after another. The Research Analyst collects facts and open questions. The Technical Writer turns that brief into a 250-word explainer. The Editor tightens it and flags anything that needs a source. Each one's output becomes the next one's input. Every one of these steps is a call to the model, which is why it takes a moment."

When it finishes, open `output/explainer.md` and read a few lines.

"Look at what the Editor did: it marked claims that need a source. Now look at the numbers in the text. This kit has no web search, so those figures come from the model's memory, and I would not trust them. I would check each one before it went into a paper. That is the most important habit when you use these tools."

Open `main.py` and scroll slowly. "The top of the file connects to the model. Then three blocks: the **agents** (who is on the team), the **tasks** (what each person owns), and the **crew** (how they work together). That is the entire design."

In `main.py`, find the Writer's `backstory` and replace it with:
```python
backstory="You write for a skeptical finance director who wants the bottom line, the risks and the cost in plain language.",
```
Save, then rerun the same command:
```
uv run python main.py "retrieval-augmented generation"
```

"I changed one sentence, the Writer's backstory. Compare the tone with the first run. The backstory is part of what the model is told about who it is, so it changes how the writing sounds. This is the same kind of small change you will make in the assignment."

**IDEA:**
"For research, you can imagine a crew that handles a first pass of a literature review: one agent summarizes each paper, one groups them by theme, one flags weak or contradictory claims. You would still read the papers that matter. The crew helps you decide where to look first."

"More agents means more model calls, so more time and more cost. And this kit has no tools, so the research is only what the model already knows."

`cd ..`

**IF IT BREAKS:** a `500` error means Groq had a hiccup, so run it again. A `429` means a rate limit: wait a minute.

---

## OpenAI Agents SDK: one agent with tools and hand-offs

"This is the OpenAI Agents SDK segment. The idea: one front-desk agent, called Triage, decides which specialist should handle your question, and the specialists can use tools. A **tool** is just a Python function that the model is allowed to call, like a calculator."

```
cd agents-sdk-kit
uv run python main.py
```

1. `What is 15% of 240, plus 38?`
   **SAY:** "See the line that says hand-off, with a reason. Triage decided this is a math question and passed it to the Math Tutor, which used its calculator tool. The label in brackets shows who actually answered."
2. `Make this sentence stronger: "We did a lot of work on the project and it went okay."`
   **SAY:** "Now the Writing Coach, which counts the words before and after. Notice something: it changed 'it went okay' to 'it succeeded.' That is a stronger claim than I made. The tool tightened my sentence and also overstated my result. In research writing, that is exactly the kind of change you must catch."
3. `What can you help me with?`
   **SAY:** "Neither specialist fits, so Triage answers by itself. Routing is a decision the model makes each time."

type `quit`.

Open `main.py`. Find the two **tools** (`calculate` and `word_count`), the two **specialist agents**, and the `triage` agent with its list of hand-offs. "A tool is a plain function with a description. A specialist is an agent with its own instructions and tools. Triage knows who the specialists are and when to pass work to them."

**WHY**
"Two details cost us time during preparation, and they are good lessons. First, when you point these frameworks at a different AI provider, check the model name that is actually sent. Second, a hand-off is itself a tool, and this provider rejected a tool with an empty description of its inputs. Our fix was to give each hand-off a small `reason` input. When something fails in a new combination of tools, the error message usually says exactly what is wrong. Read it before searching."

In `main.py`, add a third specialist above the `triage` agent:
```python
translator = Agent(
    name="Translator",
    handoff_description="Translates text into another language.",
    instructions="You translate the user's text into the language they ask for. Reply with the translation only.",
    model=MODEL,
)
```
Then add one line inside the `triage` agent's `handoffs=[...]` list, copying the style of the other two:
```python
        handoff(translator, on_handoff=show_reason, input_type=HandoffReason),
```

Save, rerun `uv run python main.py`, and type: `Translate "good morning, class" into Spanish.`

"I added a new specialist with a few lines, and Triage can now route to it. This is the pattern for building up an assistant one skill at a time."

**IDEA:**
"A research version: one front-desk agent routes your question to a statistics helper that runs a calculation tool, a writing helper that tightens your abstract, or a citation helper that formats references. Each helper is small and does one job."

`cd ..`

**IF IT BREAKS:** if the Translator does not get the request, move on.

---

## LangGraph: a flowchart you draw

"This is the LangGraph segment. The first two tools hide the loop from you. LangGraph makes you draw it. You define **nodes**, which are steps, and **edges**, which are the allowed moves between them. A shared **state**, the conversation so far, flows through the graph. Nothing happens unless you draw an arrow for it."

This graph has two nodes, agent and tools, plus the special points START and END. The edges are the arrows between them. START leads to agent. From agent there is a conditional edge: if the model asked for a tool, the graph goes to tools, and if not, it goes to END. A fixed edge always leads from tools back to agent. A node is a step, an edge is an allowed move, and a conditional edge is a choice made at run time. Here the choice is: does the agent need a tool or not?

```
cd langgraph-kit
uv run python main.py
```
The program first prints the graph as text.
Copy that text, starting at the line `graph TD;`, and paste it into **https://mermaid.live**.
"That text is the same flowchart we drew, generated from the code. The picture and the program can never disagree, because the picture comes from the program."

**DO and SAY (type each prompt):**
1. `What time is it?`
   **SAY:** "Watch the labels printed on the left. `[agent]` decided to call a tool, `[tools]` ran it, and `[agent]` wrote the answer. That is the loop."
2. `What is 17 * 23?`
3. `What time is it in UTC, and what is that hour multiplied by 60?`
   **SAY:** "A chain: the agent used two tools in sequence, then combined the results."
4. `What was my first question?`
   **SAY:** "It remembers, because the graph saves its state between turns. That is the checkpointer in the code."

type `quit`.

In the `langgraph-kit` folder, create a new file named `approve_demo.py` and paste this in:
```python
from langgraph.checkpoint.memory import MemorySaver

from main import builder  # importing main does NOT start the chat loop

graph = builder.compile(checkpointer=MemorySaver(), interrupt_before=["tools"])
config = {"configurable": {"thread_id": "t1"}}

for step in graph.stream({"messages": [("user", "What is 17 * 23?")]}, config):
    pass  # the graph stops BEFORE the tools node runs

state = graph.get_state(config)
print("Paused before:", state.next)
print("Waiting for approval:", state.values["messages"][-1].tool_calls)

input("Press Enter to approve and continue...")
for step in graph.stream(None, config):  # None = "approved, continue"
    pass
print("Answer:", graph.get_state(config).values["messages"][-1].content)
```
Run it:
```
uv run python approve_demo.py
```

"The graph stopped *before* running the tool and showed us exactly what it wanted to do. Nothing happened until I pressed Enter. This is called a human-in-the-loop checkpoint. It is one line of code, `interrupt_before`, and it is the main reason people choose LangGraph."

**IDEA:**
"In research this matters most when an agent's action has consequences: before it deletes or overwrites a data file, before it runs a long and expensive analysis, before it sends data to an outside service. You approve each step instead of trusting the whole run."

`cd ..`

**IF IT BREAKS:** if `approve_demo.py` errors, make sure it is saved inside `langgraph-kit` (next to `main.py`) and that you ran it from that folder. 

---

## Compare and Choose

"CrewAI is a team of specialists: fast to describe, good when work splits into roles. The Agents SDK is one assistant with tools and hand-offs: a light way to start. LangGraph is a flowchart you draw: the most work, and the most control, including pausing for a human.

A simple rule: start with the one that matches how you think about your problem. Move to LangGraph when you need to control exactly what happens and when, or when someone must approve a step."

**Questions:**
- "Where have you seen teams of agents used in real work, and what went wrong?"
- "If an employer asked you to build an agent, which of these three would you reach for, and why?"
- "What would a hiring manager want to see a data scientist do with this?"
- "What would worry you about running this in production: cost, reliability, safety?"

"Two things matter more than the framework. First, **tools**: an agent is only as useful as what it can do. Second, **evaluation**: keep a few test questions and rerun them after every change, so you know whether you improved the agent or broke it."

---

## The Assignment

Open `agentic_assignment.md`.

"The assignment is small. Pick **one** of the three frameworks. Run its starter kit. Make **one change of your own** that visibly changes the output, the same kind of change we just did live. Then write a short report on what you changed and how the framework works.

Five things to know:

1. **It is graded for effort.** A complete, honest attempt earns full credit, and a change that did not work still counts if you explain what happened.
2. **You do not attach files.** You build a **new, empty folder**, copy in only your changed file, your `REPORT.md`, and your two output files, share the folder, and paste the link into the **Website URL** box on Canvas.
3. **You must save two outputs:** `run1_original`, the unmodified kit, and `run2_changed`, your changed version. Each shows the command and what came back. These prove you ran it.
4. **Test your link in a private window** before you submit. If you cannot open it signed out, we cannot either.
5. **An exposed API key is an automatic zero.** Never put your `.env` file or your key in the folder, in a pasted output, or in a screenshot. If a key is ever exposed, delete it at the Groq console and make a new one."

**DO:** show the `REPORT.md` template (five short sections) and state the **due date**.

"Unzip the kits into a normal folder on your computer, such as Documents, **not** into Google Drive, OneDrive or Dropbox. Those services upload the thousands of small files in the install, which slows it down or corrupts it, and they upload your `.env` file, which contains your key."

---

## Close

"If you remember one thing: these tools exist, they are free to try today, and a useful agent is mostly a clear description of the job, the right tools, and your own checking of the result. Start small. Pick one repetitive multi-step job in your research and ask which of the three pictures fits it. Everything is in the repository, and the full guide walks through each step with the reasons."

---

## Quick reference: every command, in order

```
cd crewai-kit
uv run python main.py "retrieval-augmented generation"
cd ..

cd agents-sdk-kit
uv run python main.py
cd ..

cd langgraph-kit
uv run python main.py
uv run python approve_demo.py
cd ..
```

| If this happens | Do this |
| :--- | :--- |
| `500` error | Groq had a hiccup. Run it again |
| `429` error | Wait a minute, or set `GROQ_MODEL=openai/gpt-oss-20b` in `.env` (offscreen) |
| "model not found" | The model was retired. Check https://console.groq.com/docs/deprecations and change `GROQ_MODEL` |
| `400` error mentioning a "schema" | A tool is described incorrectly. Read the message, it names the tool |
| Wi-Fi or Groq is down | Show the saved backup outputs and the board drawings, and say so plainly |
| Output differs from the guide | Expected. Say so once, early |
| You want to stop a running program | Press `Ctrl+C` in the terminal. In a chat, type `quit` instead |
