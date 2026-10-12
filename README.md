# Agentic Workflows: Starter Kits

Three small starter kits for three ways of building agentic workflows: **CrewAI**, the **OpenAI Agents SDK**, and **LangGraph**. All three use a free Groq API key.

## Get started

1. **Download [`agentic-starter-kits.zip`](agentic-starter-kits.zip).** Open the file and use the download button on its page.
2. **Unzip it.** On Windows, right-click the zip and choose **Extract All...**; on Mac, double-click it. You will get a folder named `agentic-starter-kits`.
3. **Open that folder in VS Code** (**File → Open Folder**). The left panel should list `crewai-kit`, `agents-sdk-kit`, `langgraph-kit` and `.env.example` directly. If you only see one more folder named `agentic-starter-kits`, open that inner folder instead.
4. **Read the document that matches what you are doing:**

| If you want to... | Read |
| :--- | :--- |
| Follow along live in class (optional, about 10 minutes) | [`pre_class_setup.md`](pre_class_setup.md) |
| Run everything yourself after watching the class recording | [`agentic_workflow_guide.md`](agentic_workflow_guide.md) |
| See what to turn in, and how it is graded | [`agentic_assignment.md`](agentic_assignment.md) |
| See the script we followed in the class demo | [`presenter_run_of_show.md`](presenter_run_of_show.md) |

The first three documents are also inside the zip, so you can read them offline.

## What is in the zip

| Folder | The idea | Run it |
| :--- | :--- | :--- |
| `crewai-kit/` | A **team of role-playing agents** working through tasks | `uv run python main.py "topic"` |
| `agents-sdk-kit/` | One agent with **tools and hand-offs** to specialists | `uv run python main.py` |
| `langgraph-kit/` | An agent as an **explicit graph** you draw yourself | `uv run python main.py` |

## What you need

- [uv](https://docs.astral.sh/uv/) and VS Code, set up earlier in the course
- A free Groq API key from https://console.groq.com/keys

## One safety rule

Your API key goes in a file named `.env`. **Never share it, screenshot it, or include it in anything you submit.** The assignment explains how to build a submission that leaves it out.
