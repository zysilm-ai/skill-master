<p align="center">
  <img src="docs/architecture_v2.png" alt="Skill Master Architecture" width="700">
</p>

<h1 align="center">Skill Master</h1>

<p align="center">
  <strong>"LLM Agent + Natural Language is Turing Complete"</strong>
</p>

<p align="center">
  <a href="#overview">Overview</a> •
  <a href="#quick-start">Quick Start</a> •
  <a href="#how-it-works">How It Works</a> •
  <a href="#the-turing-machine-parallel">Philosophy</a> •
  <a href="#skills-included">Skills</a>
</p>

---

## Overview

**Skill Master** is an intelligent skill orchestrator for Claude Code that automatically **finds**, **creates**, **executes**, and **improves** skills based on actual usage.

Inspired by the **Turing Machine**, Skill Master treats skills as a "tape" that can be read, written, and modified - creating a self-improving agent system.

### Two Nested Turing Machines

The system operates as two nested Turing machines:

| Level | Tape | Head | Instructions |
|-------|------|------|--------------|
| **Agent TM** (L2) | Skill Repository | Skill Master | Search → Create → Execute → Review → Improve |
| **LLM TM** (L1) | LLM Context | LLM Model | Training + Prompts |

- **L1 (Foundation):** The LLM processes tokens in its context window
- **L2 (Agent):** Skill Master manages skills as a higher-level "tape"
- The Agent TM executes via the LLM TM, achieving Turing-completeness at both levels

### What It Does

1. **Search** - Finds existing skills (local, GitHub, web)
2. **Create** - Generates new skills through deep research when none exist
3. **Execute** - Invokes the skill to complete your task
4. **Review** - Compares execution against skill instructions (empirical, not theoretical)
5. **Improve** - Updates skills based on actual divergences

The review is **self-limiting**: if a skill executes perfectly, no improvement is needed. Skills converge toward optimal instructions through real usage.

---

## Quick Start

### Installation

Register the skill marketplace in Claude Code:

```bash
/plugin marketplace add zysilm-ai/skill-master
```

Or for local installation, clone and register:

```bash
git clone https://github.com/zysilm-ai/skill-master.git
cd skill-master
/plugin marketplace add ./
```

### Usage

Simply invoke Skill Master with your task:

```
/skill-master Create a business plan for an electric motorcycle startup
```

Or let Claude Code auto-detect based on context:

```
I need to create a comprehensive market analysis for renewable energy in Europe
```

Skill Master will:
1. Search for a matching skill
2. Create one if not found (after researching best practices)
3. Ask where to store it (local or global)
4. Execute the skill to complete your task
5. Review and offer improvements based on execution

---

## How It Works

### The Workflow

```
User Request
     │
     ▼
┌─────────────┐     ┌─────────────┐     ┌─────────────┐
│   SEARCH    │────▶│   CREATE    │────▶│   EXECUTE   │
│   Skills    │     │   if none   │     │   Skill     │
└─────────────┘     └─────────────┘     └─────────────┘
                                              │
                                              ▼
                                        ┌─────────────┐
                                        │   REVIEW    │
                                        │  (fresh agent)
                                        └─────────────┘
                                              │
                          ┌───────────────────┴───────────────────┐
                          │                                       │
                          ▼                                       ▼
                   No Divergence                            Divergence Found
                          │                                       │
                          ▼                                       ▼
                      ┌───────┐                            ┌─────────────┐
                      │ DONE  │                            │   IMPROVE   │
                      └───────┘                            │   Skill     │
                                                           └─────────────┘
```

### Empirical Review

The review phase is **empirical, not theoretical**:

- Compares what the skill **says to do** vs what **actually happened**
- Uses a **fresh agent** (via Task tool) for unbiased comparison
- Only suggests improvements when execution **diverged** from instructions
- User decides whether to apply improvements

This ensures skills improve based on **real issues**, not arbitrary criteria.

---

## The Turing Machine Parallel

### Philosophy

Just as a Turing machine's computational power comes from its **table of instructions**, an AI agent's capability comes from its **skills**.

Skill Master operates as the "head" of a higher-level Turing machine:

| Turing Machine | Skill Master System |
|----------------|---------------------|
| **Tape** | Skill Repository (can be read, written, modified) |
| **Head** | Skill Master (searches, creates, executes, improves) |
| **State Register** | Conversation state |
| **Instructions** | The skill-master workflow itself |

### Why This Matters

- **Self-Improving:** Skills get better through actual usage
- **Self-Limiting:** Well-written skills stop needing improvements
- **Composable:** Skills can invoke other skills
- **Portable:** Skills work across projects and teams

---

## Skills Included

### skill-master
The main orchestrator that manages the skill lifecycle.

### business-plan-generator
Creates comprehensive business plans with:
- Executive summary
- Market analysis
- Competitive landscape
- Financial projections
- Implementation roadmap

---

## Directory Structure

```
skill-master/
├── .claude-plugin/
│   ├── plugin.json          # Plugin metadata
│   └── marketplace.json     # Marketplace config
├── skills/
│   ├── skill-master/
│   │   ├── SKILL.md         # Main orchestrator
│   │   └── references/
│   │       ├── skill-search.md
│   │       ├── skill-create.md
│   │       ├── skill-review.md
│   │       └── skill-improve.md
│   └── business-plan-generator/
│       └── SKILL.md
├── docs/
│   ├── architecture.md
│   ├── architecture_v2.png
│   └── generate_diagram.py
└── README.md
```

---

## Configuration

Skills can be stored in two locations:

| Location | Scope | Use Case |
|----------|-------|----------|
| `.claude/skills/` | Project | Team-shared, committed to git |
| `~/.claude/skills/` | Personal | Available across all projects |

---

## Contributing

Contributions welcome! Areas for improvement:

- Additional skill templates
- Better search algorithms
- More improvement patterns
- Integration with other agent frameworks

---

## License

MIT License - See [LICENSE](LICENSE)

---

## Acknowledgments

- [Claude Code](https://claude.ai/code) - AI coding assistant
- [Anthropic](https://anthropic.com) - For the Claude model
- Alan Turing - For the theoretical foundation

---

<p align="center">
  <em>Skills are the programs that make agents Turing-complete.</em>
</p>
