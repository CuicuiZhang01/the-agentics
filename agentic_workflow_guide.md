# Agentic Workflows: Starter Kits Guide

CrewAI · OpenAI Agents SDK · LangGraph

Three frameworks, three different answers to the same question: **how do you let an LLM plan, use tools, and work toward a goal instead of answering once?** Each kit is the smallest working starting point. Your job is to get it running, understand the four or five moving parts, then change it.

> **How to use this guide.** The class session was recorded as a demonstration. Watch the recording first, then work through this guide at your own pace: set up once (Part 0), then run each kit (Parts 1 to 3). The assignment is in `agentic_assignment.md`. If you already finished the short `pre_class_setup.md`, you can skip Part 0.

| Framework | Mental model | You describe... | Reach for it when... |
| :--- | :--- | :--- | :--- |
| **CrewAI** | A team of specialists | Roles, goals, and tasks | The work splits naturally into roles with hand-offs |
| **OpenAI Agents SDK** | One agent with tools, plus hand-offs | Instructions, tools, and who to pass to | You want a lightweight agent loop with little ceremony |
| **LangGraph** | A flowchart you draw | State, nodes, and edges | You need explicit control: loops, branches, pauses, memory |

> **Expect variation.** LLMs are non-deterministic. Your output will not match anyone else's, and that is normal.

---

## Part 0: Setup (once, before you run anything; about 10 to 15 minutes)

### Step 1. Get the kits

1. **Download** `agentic-starter-kits.zip` (your instructors will share the link) and **unzip** it.
   - **Windows:** right-click the zip, choose **Extract All...**, then click **Extract**.
   - **Mac:** double-click the zip.
2. **Open the folder in VS Code.** In VS Code choose **File → Open Folder...** (on Mac, **File → Open...**), select `agentic-starter-kits`, and click **Select Folder** (on Mac, **Open**).
3. **Check that you opened the right level.** The Explorer panel on the left should list these directly: `agents-sdk-kit`, `crewai-kit`, `langgraph-kit`, and `.env.example`. If you instead see only one folder, also named `agentic-starter-kits`, you opened one level too high (Windows sometimes puts a folder inside a folder when extracting). Choose **File → Open Folder...** again and open that inner folder.
4. **Open the terminal:** **Terminal → New Terminal** (or `` Ctrl+` ``). It opens in the folder you just opened.

### Step 2. Create your `.env` file (once, for all three kits)

Get a free key at **https://console.groq.com/keys**, then:

```bash
# Mac / Linux
cp .env.example .env
```

```powershell
# Windows PowerShell
Copy-Item .env.example .env
```

Open `.env` in VS Code and replace `paste_your_key_here` with your key. Never commit this file; the included `.gitignore` already excludes it.

### Step 3. Install each kit

Each kit is its own project with its own virtual environment. `uv` creates it for you.

```bash
cd crewai-kit
uv sync
uv run python check_setup.py
cd ..
```

Repeat for the other two:

```bash
cd agents-sdk-kit
uv sync
uv run python check_setup.py
cd ..

cd langgraph-kit
uv sync
uv run python check_setup.py
cd ..
```

**What to expect**

- **CrewAI is the heavy one** (roughly 770 MB of packages). On a fast connection it takes under a minute; on slower Wi-Fi allow several minutes. Start it first.
- The other two are small (roughly 55 MB each) and install in seconds.
- `uv` may download Python 3.12 the first time. That is automatic.
- Each `check_setup.py` should end with `... kit OK`. If it does, your key, model, and install all work.

---

## Part 1: CrewAI (a team of specialists)

### Run it

```bash
cd crewai-kit
uv run python main.py "how small teams can use AI agents in data science"
```

Watch the terminal. Three agents work in sequence (Research Analyst → Technical Writer → Editor), and the final explainer is saved to `output/explainer.md`.

### How it works

Open `main.py`. There are four pieces:

| Piece | In the code | What it is |
| :--- | :--- | :--- |
| **LLM** | `llm = LLM(...)` | The model every agent uses |
| **Agent** | `Agent(role, goal, backstory)` | *Who* is on the team. The backstory becomes part of the system prompt |
| **Task** | `Task(description, expected_output, agent)` | *What* each person owns, and what "done" looks like |
| **Crew** | `Crew(agents, tasks, process)` | *How* they work together. `Process.sequential` passes each task's output to the next |

`{topic}` in the goals and descriptions is filled in by `crew.kickoff(inputs={"topic": ...})`.

**Why the odd `openai/` prefix?** CrewAI has no built-in Groq provider, but Groq speaks the OpenAI API. We point CrewAI's OpenAI client at Groq's address. The leading `openai/` only selects the client; the model name Groq receives is whatever is in `GROQ_MODEL`.

### Make it yours (pick one; these are also your options for the assignment)

1. **Change the voice.** Rewrite the Writer's backstory to target a different audience (a 10-year-old, a CFO, a skeptical reviewer). Re-run and compare the tone.
2. **Add a fourth agent.** Create a Fact Checker with its own task, and add both to the `agents` and `tasks` lists in order.
3. **Break it on purpose.** Remove `expected_output` from one task, or make two goals contradict. What changes in the result?

### Know the limits

- The researcher has **no web access** in this kit. Its "research" is the model's own knowledge, so treat facts as unverified. Giving agents tools is the natural next step.
- Every agent is one or more model calls, so more agents means more time and more cost.

---

## Part 2: OpenAI Agents SDK (an agent with tools and hand-offs)

### Run it

```bash
cd agents-sdk-kit
uv run python main.py
```

Try these prompts, one at a time, and watch the name in square brackets before each reply:

1. `What is 18% of 2450?` → routed to **Math Tutor**, which calls a calculator tool
2. `Tighten this: we utilize synergies to leverage our core competencies` → routed to **Writing Coach**
3. `What is a vector database?` → neither specialist fits, so **Triage** answers itself

Type `quit` to exit.

### How it works

The SDK's whole idea fits in one sentence: **an agent is instructions + tools + (optionally) other agents it can hand off to.**

| Piece | In the code | What it is |
| :--- | :--- | :--- |
| **Tool** | `@function_tool` on a plain Python function | Something the model can call. The docstring becomes its description |
| **Agent** | `Agent(name, instructions, tools, model)` | One role with its own tools |
| **Hand-off** | `handoffs=[math_tutor, writing_coach]` | Lets Triage pass the conversation to a specialist |
| **Runner** | `Runner.run_sync(triage, history)` | Loops: model → tool or hand-off → model... until a final answer |

Two details worth noticing:

- `result.last_agent.name` tells you who actually answered. That is your hand-off trace.
- `result.to_input_list()` returns the whole conversation so far; feeding it back in is how the agent "remembers".

**The model wrapper (a real gotcha).** The code uses `OpenAIChatCompletionsModel(...)` instead of passing a plain model string. If you pass the bare string `"openai/gpt-oss-120b"`, the SDK treats `openai/` as *its own* provider prefix and strips it, so Groq would receive `gpt-oss-120b`, which is not a valid Groq model name. The wrapper sends the name unchanged. Remember this when pointing any framework at a non-default provider: **check what model name actually goes over the wire.**

### Make it yours (pick one; these are also your options for the assignment)

1. **Add a specialist.** Create a `Translator` agent with a `handoff_description`, and add it to Triage's `handoffs`.
2. **Add a tool.** Write `reading_time(text: str) -> str` (words ÷ 200) and give it to the Writing Coach.
3. **Confuse the router.** Ask something that fits two specialists at once. Who wins, and can you fix it by rewriting the `handoff_description`s?

---

## Part 3: LangGraph (a flowchart you draw)

### Run it

```bash
cd langgraph-kit
uv run python main.py
```

First it prints the graph as **Mermaid** text. Copy everything from `graph TD;` down, paste it into **https://mermaid.live**, and you will see the picture: `agent ⇄ tools`, with a path to the end.

Then chat. Try:

1. `What is 17 * 23?`
2. `What time is it in UTC, and what is that hour multiplied by 60?` (a two-tool chain)
3. `What was my first question?` (memory across turns)

Each turn prints which node ran (`[agent]`, `[tools]`), so you can see the loop happen.

### How it works

| Piece | In the code | What it is |
| :--- | :--- | :--- |
| **State** | `class State(TypedDict)` | The data that flows through the graph. `add_messages` *appends* instead of overwriting |
| **Node** | `def agent(state)` | A function: reads state, returns an update |
| **Edge** | `add_edge("tools", "agent")` | A fixed move from one node to another |
| **Conditional edge** | `add_conditional_edges("agent", tools_condition)` | A branch: call tools, or finish |
| **Checkpointer** | `MemorySaver()` | Saves state per `thread_id`, which is what gives the chat memory |

The difference from the other two: **you** decide the control flow. Nothing happens unless there is an edge for it.

### Make it yours (pick one; these are also your options for the assignment)

1. **Add a tool.** Write a new `@tool` function and add it to the `tools` list. Nothing else changes, because the graph already knows how to run tools.
2. **Add a human approval step.** Pause before any tool runs, and resume when you say so. Change the compile line to:

   ```python
   graph = builder.compile(checkpointer=MemorySaver(), interrupt_before=["tools"])
   ```

   Then run a question once, inspect the pending call, and resume:

   ```python
   config = {"configurable": {"thread_id": "t1"}}
   for step in graph.stream({"messages": [("user", "What is 17 * 23?")]}, config):
       pass                                   # stops BEFORE the tools node
   state = graph.get_state(config)
   print(state.next)                          # ('tools',)
   print(state.values["messages"][-1].tool_calls)   # the call waiting for approval
   for step in graph.stream(None, config):    # None = "approved, continue"
       pass
   print(graph.get_state(config).values["messages"][-1].content)
   ```
3. **Add a node.** Insert a `check_answer` node between `agent` and the end that asks the model whether the answer is complete, and loops back if not.

---

## Part 4: Choosing between them

| Question | CrewAI | Agents SDK | LangGraph |
| :--- | :--- | :--- | :--- |
| How do I describe the system? | Roles + tasks | Agent + tools + hand-offs | State + nodes + edges |
| How much control do I have? | Moderate (process types) | Moderate (instructions) | **Full** (every edge is yours) |
| Setup weight | Heavy install | Light | Light |
| Lines to a first working agent | Fewest to describe a team | Few | Most, but explicit |
| Memory / pause / approval, *as shown in these kits* | Not demonstrated (check the docs) | Memory carried by hand with `to_input_list()` | Built in: checkpointer + `interrupt_before` |
| Best for | Multi-role content or analysis pipelines | Routing and tool use | Anything that must loop, branch, or be audited |

A practical rule: **prototype in whichever matches how you think about the problem, and move to LangGraph when you need to control exactly what happens when.**

---

## Troubleshooting

| Problem | Fix |
| :--- | :--- |
| `uv: command not found` | Close and reopen the terminal after installing uv |
| `KeyError: 'GROQ_API_KEY'` or "key not found" | The `.env` file is missing, misnamed (it must be `.env`, not `.env.txt`), or in the wrong place. It belongs in the `agentic-starter-kits` folder, next to `.env.example` |
| `401` / "Invalid API Key" | Re-copy your key from console.groq.com/keys; no quotes, no spaces |
| "model not found" / `404` | Groq retires models regularly. Check https://console.groq.com/docs/deprecations and change `GROQ_MODEL` in `.env`. Lighter option: `openai/gpt-oss-20b` |
| `429` / "rate limit" | Wait a minute and retry, or switch `GROQ_MODEL` to `openai/gpt-oss-20b`. Limits vary by account; see your Groq console |
| CrewAI install is slow | Normal; it is the largest package set. Start it first and keep going with the others |
| `uv sync` fails on Python version | Run `uv python install 3.12`, then `uv sync` again |
| Agent loops or gives odd answers | Smaller models and vague instructions both cause this. Tighten the instructions, or lower `temperature` |
| Output differs from a classmate's | Expected. LLM output is not deterministic |

---

## Going further

- **Give agents real tools:** web search, file reading, a database query. Tools are where agents stop being chatbots.
- **Add evaluation:** write 5 test prompts with expected behavior and run them after every change. An agent you cannot test is an agent you cannot trust.
- **Compare frameworks on one task:** build the same small task in all three and note where each one fought you.
- **More depth:** the `agentic-playground` repo has full modules on LangGraph (module 06) and CrewAI (module 08), among others.
- **Official docs:** https://docs.crewai.com and https://openai.github.io/openai-agents-python/
