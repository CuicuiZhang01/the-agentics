# Optional: Get Set Up Before Class

**You do not need to do this.** Class is a recorded demonstration, and you are not expected to code along. This sheet is only for people who want to follow along live. Everyone else can watch the recording and use `agentic_workflow_guide.md`, which contains the complete setup.

It takes about 10 minutes. You already have `uv` and a Groq key from earlier setup.

## 1. Get the kits

1. **Download** `agentic-starter-kits.zip` and **unzip** it.
   - **Windows:** right-click the zip, choose **Extract All...**, then click **Extract**.
   - **Mac:** double-click the zip.
2. **Open the folder in VS Code.** In VS Code choose **File → Open Folder...** (on Mac, **File → Open...**), select `agentic-starter-kits`, and click **Select Folder** (on Mac, **Open**).
3. **Check that you opened the right level.** The Explorer panel on the left should list these directly: `agents-sdk-kit`, `crewai-kit`, `langgraph-kit`, and `.env.example`. If you instead see only one folder, also named `agentic-starter-kits`, you opened one level too high (Windows sometimes puts a folder inside a folder when extracting). Choose **File → Open Folder...** again and open that inner folder.
4. **Open the terminal:** **Terminal → New Terminal** (or `` Ctrl+` ``). It opens in the folder you just opened.

## 2. Add your Groq key (once, for all three kits)

```bash
# Mac / Linux
cp .env.example .env
```

```powershell
# Windows PowerShell
Copy-Item .env.example .env
```

Open `.env` and replace `paste_your_key_here` with your key (free at https://console.groq.com/keys). The file must be named exactly `.env`. Windows hides file extensions by default, so check that it did not become `.env.txt`.

## 3. Install and check each kit

There are three kits, and each one is installed separately. Do them one at a time, **starting with CrewAI**, because it is the largest install and may take several minutes.

Each block below starts in the `agentic-starter-kits` folder and ends back in it (that is what `cd ..` does), so you can run the blocks one after another without moving around.

What the two commands do: `uv sync` installs everything the kit needs, and `check_setup.py` makes one tiny call to Groq to prove your key, model and install all work.

### Kit 1 of 3: CrewAI (slowest)

```bash
cd crewai-kit
uv sync
uv run python check_setup.py
cd ..
```

You should see a model name, a one-word reply, and a last line reading `CrewAI kit OK`.

### Kit 2 of 3: OpenAI Agents SDK

```bash
cd agents-sdk-kit
uv sync
uv run python check_setup.py
cd ..
```

You should see a model name, a one-word reply, and a last line reading `Agents SDK kit OK`.

### Kit 3 of 3: LangGraph

```bash
cd langgraph-kit
uv sync
uv run python check_setup.py
cd ..
```

You should see a model name, a one-word reply, and a last line reading `LangGraph kit OK`.

### You are done when

All three checks ended with their `... kit OK` line. The one-word reply may differ from kit to kit; that is normal.

If something fails, do not spend class time on it. Bring the error message and use the troubleshooting table in the full guide.

> Never share a screenshot that shows your `.env` file. It contains your key.
