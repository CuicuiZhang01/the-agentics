# Assignment: Agentic Workflow Build Report

**Individual · due [due date] · submit a link on Canvas**

## The task

Choose **one** of the three frameworks from class (CrewAI, OpenAI Agents SDK, or LangGraph). Run its starter kit, make **one meaningful change of your own**, and write a short report explaining what you did and why.

The goal is not a large project. It is to show that you can get an agent framework running, change how it behaves on purpose, and reason about when to use it.

## Steps

1. Complete setup (Part 0 of `agentic_workflow_guide.md`) and confirm your chosen kit's `check_setup.py` ends with `... kit OK`.
2. Run the unmodified kit once and save the output as **`run1_original`** (see **What to submit**).
3. Make **one change that alters the behavior you can see in the output.** Renaming a variable or rewording a comment does not count. Options:
   - **CrewAI:** add a fourth agent with its own task; or rewrite the crew for a different domain (for example, a research-to-brief team for a topic you care about)
   - **Agents SDK:** add a new specialist agent; or add a new tool and give it to an agent
   - **LangGraph:** add a new tool; or add a human-approval pause before tools run; or add a node that checks the answer before finishing
   - **Your own idea:** welcome, if it is about the same size. If unsure, ask first.
4. Run your changed version and save the output as **`run2_changed`**.
5. Write `REPORT.md` (template below).
6. Share your folder and submit the link (see **What to submit**).

## What to submit

**A link to a shared folder.** Attachments are not accepted. Put the items below in one folder, share the folder, and paste the link into the **Website URL** box on the Canvas assignment page, then click **Submit Assignment**:

- the file(s) you changed from your chosen kit (for example `main.py`),
- `REPORT.md`,
- **`run1_original`**: the output of the unmodified kit, and
- **`run2_changed`**: the output of your changed version.

Save each run as a text file (`run1_original.txt`, `run2_changed.txt`: select the terminal text, copy, and paste into a new file) or as a screenshot (`run1_original.png`, `run2_changed.png`). These two files are how we see that you ran it, so each must show the command you typed and what came back. Make sure no screenshot or text includes your key.

For CrewAI, also include a copy of `output/explainer.md` from each run (rename them `explainer_run1.md` and `explainer_run2.md`).

If you added a package, also include that kit's `pyproject.toml`.

**Build the folder from scratch, not from `agentic-starter-kits`.** Make a new, empty folder and copy only the files listed above into it. This keeps two things out of what you share:

- your **`.env` file**, which lives in the top `agentic-starter-kits` folder and contains your API key, and
- the **`.venv` folder** inside each kit, which holds hundreds of megabytes of installed packages.

**Do not include your `.env` file or any API key**, in any file, pasted output, or screenshot. **An exposed API key is an automatic 0.** If a key is ever exposed, delete it at console.groq.com and create a new one right away.

### How to share the folder

Use whichever you already have:

- **Google Drive:** right-click the folder, choose **Share**, set General access to **Anyone with the link**, set the role to **Viewer**, then choose **Copy link**.
- **OneDrive:** right-click the folder, choose **Share**, pick a setting that lets your instructor open it (such as **Anyone with the link can view**), then choose **Copy link**.
- **GitHub:** create a **public** repository from the new folder and submit its link. Run `git status` before every push to confirm `.env` is not listed. (Git setup is in `02_git_setup.md`.)

If your account does not allow "anyone with the link", share the folder with [instructor email] instead.

**Test your link before you submit it.** Open it in a private or incognito window, where you are not signed in. If you cannot open it there, we cannot either, and there will be nothing to grade.

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

## Grading

This assignment is **graded for effort**. A complete, honest attempt earns full credit. "Complete" means:

- your link opens,
- `run1_original` and `run2_changed` are both in the folder and show a real run,
- you made a genuine attempt at a change of your own, and
- every section of `REPORT.md` is filled in.

The one exception is an exposed API key, which is an automatic 0 (see **What to submit**).

A change that did not work still counts, as long as you explain what happened and what you learned in Section 3 of your report.
