# claude-plugins

A collection of Claude Code skills and agents that extend Claude's capabilities for advanced AI workflows — prompt engineering, multi-agent system design, code improvement, and iterative self-improvement.

These tools were developed to close the gap between "asking Claude a question" and "deploying Claude as a professional-grade AI system." Each plugin encodes months of prompt engineering best practice into a reusable, invokable tool.

---

## Table of Contents

- [What's included](#whats-included)
- [Why these were built](#why-these-were-built)
- [Repository layout](#repository-layout)
- [Requirements](#requirements)
- [Quick start (recommended)](#quick-start-recommended)
- [Installation — step by step](#installation--step-by-step)
- [Usage guide](#usage-guide)
- [Using these prompts with other AI platforms](#using-these-prompts-with-other-ai-platforms)
- [Configuration](#configuration)
- [For contributors: branching and pull requests](#for-contributors-branching-and-pull-requests)
- [License](#license)

---

## What's included

### Skills

Skills are invoked with a slash command (e.g. `/prompt-engineer`). They run in your current Claude Code session, on the active model.

| Skill | Command | What it does |
|---|---|---|
| **prompt-engineer** | `/prompt-engineer` | Collaboratively designs high-performance AI prompts. Covers persona, format, constraints, edge cases, hallucination mitigation, and self-evaluation. Includes a stress-test mode and a deep-dive reference guide covering model selection and temperature tuning. |
| **swarm-architect** | `/swarm-architect` | Designs complete multi-agent systems — agent rosters, topology (pipeline/parallel/hierarchical/mesh), handoff contracts, per-agent prompts, and a swarm health checklist. Includes domain templates for finance, legal, healthcare, research, and more. |
| **code-researcher** | `/code-researcher` | Iteratively analyzes and improves a file, directory, or workflow. Identifies bottlenecks, bugs, and inefficiencies, applies targeted fixes, validates results, and logs findings. |
| **knowledge-compressor** | `/knowledge-compressor` | Compresses large, multi-source documentation (technical specs, standards, policies) into a machine-readable Logic Blueprint. Outputs four structured vaults — Governance Gate (rules), Quantitative Engine (formulas/KPIs), Procedural Workflow (Mermaid.js logic), and Semantic Glossary — using the most token-efficient encoding per vault. |

### Agents

Agents are autonomous subprocesses. They are triggered by context (you describe what you want in plain language) and run independently — including on a different model from your main session.

| Agent | How to trigger | Model | What it does |
|---|---|---|---|
| **improve** | *"improve src/foo.py"* or *"improve all skills"* | **Opus** | Iteratively improves code, processes, or Claude skills. Analyzes the target, proposes ranked changes, applies them, validates outcomes, and documents lessons learned. |

---

## Why these were built

### `prompt-engineer`
Writing a good prompt once is easy. Writing one that holds up under adversarial inputs, model updates, and edge cases — at scale, across domains — is hard. This skill encodes a professional prompt engineering methodology: modular structure, sandwich constraints, mid-point checkpoints, hallucination mitigation, and cultural safeguards. It treats prompts as empirical artifacts with version logs, not one-shot text.

### `swarm-architect`
Single-model chains break down on complex, multi-domain tasks. This skill exists to answer the question: *"How do I design a team of AI agents that doesn't fall apart?"* It enforces single-responsibility per agent, explicit handoff contracts, inter-agent conflict protocols, and a health checklist — making multi-agent systems reviewable and maintainable rather than magical.

### `code-researcher`
Claude can review code in conversation, but it loses context, doesn't validate, and doesn't document. This skill adds the missing loop: analyze → hypothesize → modify → validate → log. It's meant to be run on a file or workflow the way you'd run a senior engineer through a code review.

### `knowledge-compressor`
Complex domains — cybersecurity frameworks, project management standards, compliance requirements — are documented in sprawling, human-readable prose that is expensive to feed into an AI system. This skill solves the token problem: it strips narrative, resolves conflicting source values into ranges, and reconstructs the knowledge as hard rules, formulas, and decision logic in YAML, JSON, or Mermaid.js. The output is designed to be pasted directly into another AI prompt as a dense, authoritative context block.

### `improve` (agent)
A general-purpose improvement loop that runs on Opus for maximum reasoning depth. Unlike `/code-researcher`, it also handles Claude skills themselves — it can audit and rewrite other plugins in this repo. It was built so that the toolset can self-improve rather than requiring manual maintenance.

---

## Repository layout

```
claude-plugins/
│
├── skills/                         # Slash-command skills
│   ├── prompt-engineer/
│   │   └── SKILL.md                # Full prompt engineering methodology
│   ├── swarm-architect/
│   │   ├── SKILL.md                # Multi-agent design system
│   │   └── references/
│   │       ├── domain-rosters.md   # Pre-built agent teams by domain
│   │       └── swarm-topologies.md # Topology diagrams and handoff templates
│   ├── code-researcher/
│   │   └── SKILL.md                # Code and process improvement loop
│   └── knowledge-compressor/
│       └── SKILL.md                # Semantic compression engine (four vaults)
│
├── plugins/                        # Plugin-style agents (support model selection)
│   └── improve/
│       ├── agents/
│       │   └── improve.md          # Opus-powered improvement agent
│       └── config.json
│
├── README.md
├── .gitignore
└── LICENSE
```

**Skills vs plugins:** Skills live in `~/.claude/skills/` and are invoked via `/skill-name`. Plugins live in `~/.claude/plugins/` and support additional features like specifying which model the agent runs on. The `improve` agent uses the plugin format specifically so it can be pinned to Opus.

---

## Requirements

- **Claude Code** — CLI or desktop app ([download here](https://claude.ai/code))
- **macOS or Linux** (Windows support via WSL)
- **Git** — to clone and update the repo
- **Obsidian** + MCP filesystem server (optional — for vault saves in `prompt-engineer` and `swarm-architect`)

---

## Quick start (recommended)

This is the fastest path. Run all of this in your terminal:

```bash
# 1. Clone the repo
git clone https://github.com/charliedelph/claude-plugins.git ~/claude-plugins

# 2. Install skills
ln -s ~/claude-plugins/skills/prompt-engineer ~/.claude/skills/prompt-engineer
ln -s ~/claude-plugins/skills/swarm-architect ~/.claude/skills/swarm-architect
ln -s ~/claude-plugins/skills/code-researcher ~/.claude/skills/code-researcher
ln -s ~/claude-plugins/skills/knowledge-compressor ~/.claude/skills/knowledge-compressor

# 3. Install the improve agent
ln -s ~/claude-plugins/plugins/improve ~/.claude/plugins/improve
```

Then restart Claude Code. You're done.

> **Why symlinks?** Symlinking means that when you run `git pull` in `~/claude-plugins`, your skills update automatically — no re-copying needed.

---

## Installation — step by step

### Step 1: Install Git (if you don't have it)

```bash
# Check if you have it
git --version

# macOS: install via Homebrew if needed
brew install git
```

### Step 2: Clone this repository

```bash
git clone https://github.com/charliedelph/claude-plugins.git ~/claude-plugins
```

This downloads the repo into a folder called `claude-plugins` in your home directory. You can choose a different location — just update the paths in the steps below.

### Step 3: Make sure your Claude directories exist

```bash
mkdir -p ~/.claude/skills
mkdir -p ~/.claude/plugins
```

### Step 4: Install the skills

**Option A — Symlink (recommended):** Changes to the repo are picked up automatically.

```bash
ln -s ~/claude-plugins/skills/prompt-engineer ~/.claude/skills/prompt-engineer
ln -s ~/claude-plugins/skills/swarm-architect ~/.claude/skills/swarm-architect
ln -s ~/claude-plugins/skills/code-researcher ~/.claude/skills/code-researcher
```

**Option B — Copy:** Use this if you want to customize a skill without affecting the repo.

```bash
cp -r ~/claude-plugins/skills/prompt-engineer ~/.claude/skills/
cp -r ~/claude-plugins/skills/swarm-architect ~/.claude/skills/
cp -r ~/claude-plugins/skills/code-researcher ~/.claude/skills/
cp -r ~/claude-plugins/skills/knowledge-compressor ~/.claude/skills/
```

### Step 5: Install the improve agent

```bash
# Symlink (recommended)
ln -s ~/claude-plugins/plugins/improve ~/.claude/plugins/improve

# Or copy
cp -r ~/claude-plugins/plugins/improve ~/.claude/plugins/
```

### Step 6: Restart Claude Code

Quit and reopen the app, or restart the CLI. Claude Code loads skills and plugins at startup.

### Staying up to date

```bash
cd ~/claude-plugins && git pull
```

If you used symlinks, that's all you need. If you copied, re-run the copy commands.

---

## Usage guide

### `/prompt-engineer`

```
# Collaborative mode — Claude asks you questions and builds iteratively
/prompt-engineer

# Provide a description upfront to skip the intro
/prompt-engineer build a prompt for summarizing legal contracts

# Fast-track — Claude takes creative license and explains its choices
/prompt-engineer fast-track: sentiment classifier for customer reviews
```

After building the prompt, Claude will offer:
- **Stress-test** — runs adversarial, edge-case, and trick inputs through the prompt
- **Deep dive** — model selection matrix, temperature tuning guide, and prompt pattern library

---

### `/swarm-architect`

```
# Collaborative mode — design a swarm step by step
/swarm-architect

# Provide a goal upfront
/swarm-architect build a research swarm that synthesizes academic papers into a report

# Fast-track
/swarm-architect fast-track: financial analysis pipeline
```

Output includes:
- Agent roster table (name, role, inputs, outputs)
- Topology diagram
- Per-agent prompts with handoff contracts
- Swarm health checklist
- Orchestration framework recommendations (Claude Agent SDK, LangGraph, CrewAI, etc.)

---

### `/code-researcher`

```
# Analyze and improve a file
/code-researcher src/agents/ingestion.py

# Analyze a whole directory
/code-researcher src/agents/

# Review a process or workflow by description
/code-researcher our nightly data ingestion pipeline
```

---

### `/knowledge-compressor`

```
# Compress a named standard or framework
/knowledge-compressor NIST CSF

/knowledge-compressor PMBOK 7th Edition

/knowledge-compressor SOC 2 Type II

# Paste content directly after the command
/knowledge-compressor [paste your document text here]

# Combine multiple sources
/knowledge-compressor OWASP Top 10 + NIST 800-53
```

Output is structured into four machine-readable vaults:
- **Governance Gate** — mandatory/forbidden rules with impact levels and violation triggers
- **Quantitative Engine** — formulas, KPIs, and thresholds; conflicting values encoded as `[MIN..MAX]` ranges
- **Procedural Workflow** — decision logic in Mermaid.js; linear steps in YAML
- **Semantic Glossary** — one-sentence technical definitions, no elaboration

The compressed output is designed to be pasted directly into another AI prompt as a high-density context block, reducing token cost while preserving all actionable logic.

> **Model recommendation:** This skill processes large volumes of source documentation simultaneously. Models with large context windows are strongly recommended when compressing multiple or lengthy documents:
> - **Gemini 1.5 Pro / 2.0 Flash / 2.5 Pro** (1M+ token context) — best choice for multi-document compression
> - **GPT-4o** with extended context — good alternative
> - **Claude Sonnet / Opus** — works well for single documents or shorter inputs; may struggle with very large multi-source jobs
>
> When in doubt, run this skill's prompt directly in [Google AI Studio](https://aistudio.google.com) for maximum context headroom.

---

### `improve` agent

Say it naturally — Claude will route to the Opus-powered agent automatically:

```
improve src/agents/ingestion.py
improve the swarm-architect skill
improve all skills
run the improve agent on our deployment process
```

---

## Using these prompts with other AI platforms

The `/skill-name` commands and the `improve` agent are Claude Code features — they rely on how Claude Code loads and invokes SKILL.md files. But the **prompt content inside each skill is plain text** and works with any capable LLM. You can copy the body of any SKILL.md and paste it directly into ChatGPT, Gemini, or a local model.

This is especially useful for `knowledge-compressor`, where Gemini's 1M+ token context window is a significant advantage over Claude's smaller window.

---

### Using with Gemini (recommended for knowledge-compressor)

**Google AI Studio (free, no setup):**

1. Go to [aistudio.google.com](https://aistudio.google.com)
2. Click **Create new prompt** → **System instructions**
3. Open `skills/knowledge-compressor/SKILL.md` from this repo, copy everything below the `---` frontmatter divider, and paste it into the System Instructions box
4. In the main prompt field, type your document or domain (e.g. `NIST CSF`) and run it
5. Use **Gemini 1.5 Pro** or **Gemini 2.5 Pro** for maximum context window

**Gemini API (programmatic):**

```python
import google.generativeai as genai

genai.configure(api_key="YOUR_API_KEY")

with open("skills/knowledge-compressor/SKILL.md") as f:
    skill = f.read()
# Strip the YAML frontmatter (lines between the --- markers)
system_prompt = skill.split("---", 2)[2].strip()

model = genai.GenerativeModel(
    model_name="gemini-1.5-pro",
    system_instruction=system_prompt
)
response = model.generate_content("NIST CSF")
print(response.text)
```

---

### Using with ChatGPT / GPT-4o

**ChatGPT (browser):**

1. Go to [chatgpt.com](https://chatgpt.com) and start a new chat
2. Open the SKILL.md file for the skill you want to use
3. Copy everything below the `---` frontmatter divider
4. Paste it as your first message, prefixed with: `"Please adopt the following role and instructions:"`
5. Send a follow-up message with your actual request (e.g. `"Now compress: OWASP Top 10"`)

**Custom GPT (persistent setup):**

1. Go to [chatgpt.com/gpts/editor](https://chatgpt.com/gpts/editor)
2. Paste the skill body into the **Instructions** field
3. Save and use it as a reusable GPT — no re-pasting needed

**OpenAI API (programmatic):**

```python
from openai import OpenAI

with open("skills/knowledge-compressor/SKILL.md") as f:
    skill = f.read()
system_prompt = skill.split("---", 2)[2].strip()

client = OpenAI(api_key="YOUR_API_KEY")
response = client.chat.completions.create(
    model="gpt-4o",
    messages=[
        {"role": "system", "content": system_prompt},
        {"role": "user", "content": "NIST CSF"}
    ]
)
print(response.choices[0].message.content)
```

---

### Using locally (Ollama / LM Studio)

Running a model on your own machine gives you full privacy and no API costs. For best results with these skills, use a model with at least 7B parameters. For `knowledge-compressor`, 70B+ or a quantized Mixtral/Llama 3 is recommended.

**Ollama:**

```bash
# Install Ollama: https://ollama.com
# Pull a model
ollama pull llama3.1:70b

# Run with a skill as the system prompt
SYSTEM=$(awk '/^---/{n++; if(n==2){found=1; next}} found{print}' skills/knowledge-compressor/SKILL.md)
ollama run llama3.1:70b "$SYSTEM

User: NIST CSF"
```

**LM Studio:**

1. Download and open [LM Studio](https://lmstudio.ai)
2. Download a model (Llama 3.1 70B, Mixtral 8x7B, or similar)
3. Go to the **Chat** tab → click the system prompt field
4. Paste the skill body (everything below the frontmatter `---`)
5. Chat normally

**Ollama API (programmatic):**

```python
import requests, json

with open("skills/knowledge-compressor/SKILL.md") as f:
    skill = f.read()
system_prompt = skill.split("---", 2)[2].strip()

response = requests.post("http://localhost:11434/api/chat", json={
    "model": "llama3.1:70b",
    "messages": [
        {"role": "system", "content": system_prompt},
        {"role": "user", "content": "NIST CSF"}
    ],
    "stream": False
})
print(response.json()["message"]["content"])
```

---

### Platform comparison

| Platform | Best for | Context window | Cost |
|---|---|---|---|
| **Claude Code** (`/skill-name`) | Native integration, agent workflows | 200K tokens | Claude subscription |
| **Gemini AI Studio** | Large document compression | 1M+ tokens | Free tier available |
| **ChatGPT / Custom GPT** | Persistent reusable setups | 128K tokens | ChatGPT Plus |
| **OpenAI API** | Programmatic use, automation | 128K tokens | Pay per token |
| **Ollama / LM Studio** | Privacy, no cost, offline use | Model-dependent | Free |

---

## Configuration

### Obsidian vault integration (optional)

`prompt-engineer` and `swarm-architect` can save their final outputs to an [Obsidian](https://obsidian.md) vault. This requires the `obsidian` MCP filesystem server configured in your Claude Code settings.

To enable it, update the vault paths in the SKILL.md files to match your vault location:

- `skills/prompt-engineer/SKILL.md` → change `~/Documents/ObsidianVault/` to your vault path
- `skills/swarm-architect/SKILL.md` → same

If you don't use Obsidian, ignore this — the vault save step will simply not fire.

### `improve` agent — lessons log path

The `improve` agent writes findings to `tasks/lessons.md` in your working directory. This follows a project convention from `CLAUDE.md`-driven workflows. If your projects use a different structure, edit the path in `plugins/improve/agents/improve.md`.

---

## For contributors: branching and pull requests

New to Git? Here's how to propose a change without breaking anything.

### Fork the repo (your own copy on GitHub)

1. Go to [github.com/charliedelph/claude-plugins](https://github.com/charliedelph/claude-plugins)
2. Click **Fork** (top right) — this creates your own copy at `github.com/YOUR_USERNAME/claude-plugins`
3. Clone your fork:

```bash
git clone https://github.com/YOUR_USERNAME/claude-plugins.git ~/claude-plugins
```

### Create a branch for your change

Never edit `main` directly. Create a named branch:

```bash
cd ~/claude-plugins
git checkout -b my-improvement
```

### Make your changes, then commit

```bash
# Edit files, then:
git add .
git commit -m "Brief description of what you changed"
```

### Push to GitHub and open a pull request

```bash
git push origin my-improvement
```

Then go to your fork on GitHub — you'll see a prompt to open a Pull Request. Describe what you changed and why.

### Keeping your fork in sync

```bash
# Add the original repo as "upstream" (one-time setup)
git remote add upstream https://github.com/charliedelph/claude-plugins.git

# Pull in updates from the original
git fetch upstream
git merge upstream/main
```

---

## License

MIT — use freely, modify freely, attribution appreciated.
