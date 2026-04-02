# claude-plugins

A collection of Claude Code skills and agents for advanced AI workflows.

## What's included

### Skills (invoked via slash commands)

| Skill | Command | Description |
|---|---|---|
| **prompt-engineer** | `/prompt-engineer` | Co-creates, architects, and stress-tests high-performing AI prompts. Supports collaborative and fast-track modes. |
| **swarm-architect** | `/swarm-architect` | Designs multi-agent systems — full rosters, topologies, handoff contracts, and per-agent prompts. |
| **code-researcher** | `/code-researcher` | Iteratively researches, analyzes, and improves code or processes. |

### Agents (auto-triggered by context)

| Agent | Trigger | Model | Description |
|---|---|---|---|
| **improve** | Say "improve X" or "run the improve agent on X" | Opus | Iteratively improves code, processes, and skills. Analyzes, proposes changes, applies them, and documents lessons. |

---

## Installation

Clone the repo and symlink (or copy) into your `~/.claude` directory:

```bash
git clone https://github.com/charliedelph/claude-plugins.git ~/claude-plugins
```

### Install skills

```bash
# Symlink each skill into ~/.claude/skills/
ln -s ~/claude-plugins/skills/prompt-engineer ~/.claude/skills/prompt-engineer
ln -s ~/claude-plugins/skills/swarm-architect ~/.claude/skills/swarm-architect
ln -s ~/claude-plugins/skills/code-researcher ~/.claude/skills/code-researcher
```

### Install the improve agent

```bash
# Symlink the plugin into ~/.claude/plugins/
ln -s ~/claude-plugins/plugins/improve ~/.claude/plugins/improve
```

Or copy if you prefer not to symlink:

```bash
cp -r ~/claude-plugins/skills/prompt-engineer ~/.claude/skills/
cp -r ~/claude-plugins/skills/swarm-architect ~/.claude/skills/
cp -r ~/claude-plugins/skills/code-researcher ~/.claude/skills/
cp -r ~/claude-plugins/plugins/improve ~/.claude/plugins/
```

Restart Claude Code after installing.

---

## Configuration

### Obsidian vault integration (optional)

`prompt-engineer` and `swarm-architect` can save outputs to an Obsidian vault. This requires the `obsidian` MCP filesystem server configured in your Claude Code settings.

If you use Obsidian, update the vault paths in each SKILL.md to match your vault location:
- `prompt-engineer`: saves to `{vault}/Research/prompts/`
- `swarm-architect`: saves to `{vault}/Projects/swarms/`

If you don't use Obsidian, the vault save step is safely ignorable — the skills still work end-to-end without it.

### improve agent — lessons file

The `improve` agent logs findings to `tasks/lessons.md` in your working directory. This follows a CLAUDE.md-driven project convention. If your projects use a different structure, update the path in `plugins/improve/agents/improve.md`.

---

## Usage

```
# Prompt engineering
/prompt-engineer build a prompt for summarizing legal contracts

# Fast-track mode
/prompt-engineer fast-track: sentiment classifier for customer feedback

# Swarm design
/swarm-architect build a research swarm that synthesizes academic papers

# Code improvement
improve src/agents/ingestion.py

# Skill self-improvement
improve all skills
```

---

## Requirements

- [Claude Code](https://claude.ai/code) CLI or desktop app
- Obsidian + MCP filesystem server (optional, for vault saves)

---

## License

MIT
