# Assignment: Agentic Workflow Build Report

**Individual · due [due date] · submit on Canvas**

## The task

Choose **one** of the three frameworks from class (CrewAI, OpenAI Agents SDK, or LangGraph). Run its starter kit, make **one meaningful change of your own**, and write a short report explaining what you did and why.

The goal is not a large project. It is to show that you can get an agent framework running, change how it behaves on purpose, and reason about when to use it.

## Steps

1. Complete setup (Part 0 of `agentic_workflow_guide.md`) and confirm your chosen kit's `check_setup.py` ends with `... kit OK`.
2. Run the unmodified kit once and keep the output.
3. Make **one change that alters the behavior you can see in the output.** Renaming a variable or rewording a comment does not count. Options:
   - **CrewAI:** add a fourth agent with its own task; or rewrite the crew for a different domain (for example, a research-to-brief team for a topic you care about)
   - **Agents SDK:** add a new specialist agent; or add a new tool and give it to an agent
   - **LangGraph:** add a new tool; or add a human-approval pause before tools run; or add a node that checks the answer before finishing
   - **Your own idea:** welcome, if it is about the same size. If unsure, ask first.
4. Run your changed version and keep the output.
5. Write `REPORT.md` (template below).
6. Submit.

## What to submit

**One zip file**, uploaded to Canvas, containing:

- the file(s) you changed from your chosen kit (for example `main.py`),
- `REPORT.md`,
- the terminal output (pasted into the report, or screenshots) from **both** runs.

If you added a package, also include that kit's `pyproject.toml`.

**Build the zip from a new, empty folder, not from `agentic-starter-kits`.** Copy only the files listed above into the new folder, then zip that folder. This keeps two things out of your submission:

- your **`.env` file**, which lives in the top `agentic-starter-kits` folder and contains your API key, and
- the **`.venv` folder** inside each kit, which holds hundreds of megabytes of installed packages and is too large to upload.

**Do not include your `.env` file or any API key.** If a key is ever exposed, delete it at console.groq.com and create a new one.

*Prefer GitHub?* A link to a **public** repository is also accepted. Create the repository from the top-level `agentic-starter-kits` folder so the included `.gitignore` protects your key, and run `git status` before every push to confirm `.env` is not listed. (Git setup is in `02_git_setup.md`.)

## `REPORT.md` template

```markdown
# Agentic Workflow Build Report

**Name:** 
**Framework:** CrewAI / Agents SDK / LangGraph

## 1. What I changed
What you changed and why you chose it. Name the specific files and parts of the
code you edited. (about 100 to 200 words)

## 2. How the framework works
Explain, in your own words, how this framework produces the behavior you saw,
using its own terms (for example: agent / task / crew; tool / hand-off / runner;
state / node / edge). Show that you understand why your change worked.
(about 150 to 250 words)

## 3. Evidence
Paste the output of the original run and the changed run. Then say one thing
that differed, surprised you, or went wrong, and what you did about it.

## 4. Choosing a framework
Why does this framework fit your change? Name one of the other two and say what
would have been easier or harder with it, based on what you saw in class or in
the guide. (about 150 to 250 words)

## 5. Verification and AI use
How did you check that the output was correct or sensible? State any AI
assistance you used (including for code) and how you verified it. If you used
none, say so.
```

## Rubric (100 points)

| Criterion | Exemplary | Proficient | Developing | Beginning |
| :--- | :--- | :--- | :--- | :--- |
| **1. Kit runs, with evidence** (15) | **14 to 15:** Both runs shown; output clearly belongs to the stated framework | **11 to 13:** Both runs shown, with minor gaps | **6 to 10:** One run missing, or output unclear | **0 to 5:** No evidence it ran |
| **2. The change** (30) | **27 to 30:** A substantive change that visibly alters behavior, works, and shows original thought | **21 to 26:** A real change that works, but small or close to the class examples | **12 to 20:** Change is cosmetic, partly working, or hard to see in the output | **0 to 11:** No real change |
| **3. Explains how it works** (20) | **18 to 20:** Accurate, uses the framework's terms correctly, and explains *why* the change produced the result | **14 to 17:** Mostly accurate, some terms vague | **8 to 13:** Describes what the code does without explaining why | **0 to 7:** Missing, or inaccurate |
| **4. Framework choice** (20) | **18 to 20:** Specific reasons tied to the change; compares a second framework with real detail | **14 to 17:** Reasonable reasons; comparison is general | **8 to 13:** Generic ("it was easier") with little comparison | **0 to 7:** Missing |
| **5. Verification, AI disclosure, key safety** (15) | **14 to 15:** States how output was checked; AI use disclosed clearly; no secrets in the submission | **11 to 13:** Disclosure present; verification thin | **6 to 10:** Vague disclosure or no verification | **0 to 5:** No disclosure, or an API key is exposed in the submission |

**Notes for graders**

- An exposed API key caps criterion 5 at the Beginning band regardless of the rest. Tell the student to delete and replace the key.
- Output varies run to run. Grade whether the output reflects the change, not whether it matches the guide.
- A change that failed but is honestly diagnosed in Section 3 can still earn most of criterion 3 and criterion 5.
- The rubric totals 100, so it converts to any Canvas point value by simple scaling.
