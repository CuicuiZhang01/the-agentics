# Assignment: CrewAI Build Report

**Individual · submit a link on Canvas**

## The task

Run the CrewAI starter kit, make **one meaningful change of your own**, and write a short report on what you did and why.

The goal is not a large project. It is to show that you can get an agent framework running, change how it behaves on purpose, and reason about when to use it. You only need the CrewAI kit for this assignment. The other two kits are optional practice.

## Part 1: Do the work

1. **Set up.** Follow Part 0 of `agentic_workflow_guide.md` for the CrewAI kit, and confirm `check_setup.py` ends with `CrewAI kit OK`.
2. **Make your submission folder.** Create an empty folder named **`submission`** somewhere easy to find, such as your Documents folder. Everything you hand in goes in this folder, and nothing else.
3. **Run the unmodified kit once**, with any topic you like:
   ```
   cd crewai-kit
   uv run python main.py "your topic"
   ```
4. **Save that run into your `submission` folder** (details under **How to save your runs**):
   - the terminal text, as **`run1_original.txt`**
   - a copy of `output/explainer.md`, as **`explainer_run1.md`**

   Do this *before* the next run, because the next run overwrites `explainer.md`.
5. **Make one change that alters the behavior you can see in the output.** Renaming a variable or rewording a comment does not count. Options:
   - **Change the voice:** rewrite the Writer's backstory for a very different audience (a 10-year-old, a CFO, a skeptical reviewer).
   - **Add a fourth agent:** for example a Fact Checker with its own task, added to the `agents` and `tasks` lists in order.
   - **Change the domain:** rewrite the crew for a topic or kind of work from your own field (for example, a research-to-brief team).
   - **Break it on purpose:** remove an `expected_output` or make two goals contradict, and explain what changed.
   - **Your own idea:** welcome, if it is about the same size. If unsure, ask first.
6. **Run your changed version** with the **same topic** as before, so you can compare. Save it into `submission` as **`run2_changed.txt`** and **`explainer_run2.md`**.
7. **Copy your changed `main.py`** from `crewai-kit` into `submission`. Copy it; do not move it.
8. **Write `REPORT.md`** (template at the end of this page) and save it in `submission`.

When you are done, your `submission` folder holds these files, and only these:

```
submission/
  REPORT.md
  main.py
  run1_original.txt
  run2_changed.txt
  explainer_run1.md
  explainer_run2.md
```

If you added a package, also copy in `crewai-kit/pyproject.toml`.

## How to save your runs

**The terminal text** (`run1_original.txt` and `run2_changed.txt`): in the VS Code terminal, right-click and choose **Select All**, then right-click and choose **Copy**. Choose **File → New Text File**, paste, and save it in your `submission` folder with the file name above. The text must show the command you typed and what came back. This is how we see that you ran it.

**The explainer** (`explainer_run1.md` and `explainer_run2.md`): after each run, open `crewai-kit/output/explainer.md`, choose **File → Save As...**, and save a copy in your `submission` folder with the new name. Leave the original `output` folder where it is.

**Check before you upload:** open each file and make sure none of them shows your API key. It should not appear in these outputs, but look.

## Part 2: Submit

You submit a **link to your own GitHub copy** of the class repository. You do not attach files to Canvas.

You need a free GitHub account. If you do not have one, create one at github.com. If you cannot use GitHub for any reason, email [instructor email] before the due date.

1. **Fork the class repository.** Open [repository link], click **Fork** (top right), then **Create fork**. This makes your own copy, under your own username, that you are allowed to add files to.
2. **Open the upload page in your fork.** Click **Add file → Upload files**.
3. **Drag your `submission` folder** from File Explorer (Windows) or Finder (Mac) onto the upload box. Drag the **folder itself**, not the files inside it. You should see your files listed with `submission/` in front of each name.
4. **Click Commit changes.**
5. **Check your fork.** Open it and click into the `submission` folder. You should see your six files. The address in your browser's address bar should contain your username, not your instructor's.
6. **Submit on Canvas.** Paste that address into the **Website URL** box on the assignment page, then click **Submit Assignment**.

If dragging the folder does not work in your browser, create the folder on GitHub instead: click **Add file → Create new file**, type `submission/REPORT.md` as the name (typing the slash creates the folder), paste your report, and commit. Then open that folder and use **Add file → Upload files** to drag in the other files.

**Test your link before you submit it.** Open it in a private or incognito window, where you are not signed in. You should see your `submission` folder with your files in it. If you cannot open it there, we cannot either.

**Never upload your `.env` file or any API key**, in any file, pasted output or screenshot. **An exposed API key is an automatic 0.** If a key is ever exposed, delete it at console.groq.com and create a new one right away.

**Only drag in your `submission` folder.** Never drag in `agentic-starter-kits` or a kit folder such as `crewai-kit`. The kit folders contain a `.venv` folder with hundreds of megabytes of installed packages, and the top folder contains your `.env` file with your key.

## `REPORT.md` template

```markdown
# CrewAI Build Report

**Name:** 
**Topic I used:** 

## 1. What I changed
What you changed and why you chose it. Name the specific file and the part of
the code you edited. (about 100 to 200 words)

## 2. How CrewAI works
Explain, in your own words, how CrewAI produces the behavior you saw, using its
own terms (agent, task, crew). Show that you understand why your change worked.
(about 150 to 250 words)

## 3. Evidence
Describe how the two runs differ, and refer to your two output files. Then say
one thing that surprised you or went wrong, and what you did about it.

## 4. Choosing a framework
Why would you pick CrewAI for a project like yours? Name one of the other two
frameworks from class (the Agents SDK or LangGraph) and say what would have been
easier or harder with it, based on what you saw in class or in the guide.
(about 150 to 250 words)

## 5. Verification and AI use
How did you check that the output was correct or sensible? (Remember that the
kit has no web access, so facts in the output come from the model's memory.)
State any AI assistance you used, including for code, and how you verified it.
If you used none, say so.
```
