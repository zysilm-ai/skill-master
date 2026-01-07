---
name: skill-automata
description: "Intelligent skill orchestrator that automatically finds, creates, executes, and improves skills. When you need to accomplish a task, this skill searches for existing skills (internal, GitHub via MCP, web), creates new skills if none found, executes them, and reviews execution to improve skills based on actual usage. Use when you want automated skill discovery and continuous improvement."
allowed-tools: Read, Write, Edit, Glob, Grep, WebSearch, WebFetch, Task, AskUserQuestion, TodoWrite
---

# Skill Automata

An intelligent orchestrator that automates the entire skill lifecycle: discovery, creation, execution, and improvement.

## Overview

When invoked with a task, this skill:
1. **Searches** for existing skills that can handle the task
2. **Creates** a new skill if none found (after deep research)
3. **Executes** the skill to complete the user's task
4. **Reviews** the execution and improves the skill if needed

## Workflow

### Phase 1: Skill Discovery

**Goal**: Find an existing skill that can handle the user's task.

#### Step 1.1: Parse the Request

Identify from the user's request:
- **Core task**: What needs to be accomplished?
- **Domain**: What area/field does this belong to?
- **Keywords**: Key terms for searching

#### Step 1.2: Search for Skills

Follow the search workflow in [references/skill-search.md](references/skill-search.md).

Search order:
1. **Internal skills**: Check `~/.claude/skills/` and `.claude/skills/`
2. **GitHub**: Use GitHub MCP server to search repositories
3. **Web**: Use WebSearch for broader discovery

#### Step 1.3: Evaluate Results

**If skill found:**
- Present the skill to user with description
- Proceed to Phase 2 (Storage Confirmation)

**If NO skill found:**
- Inform user: "No existing skill found. I'll research and create one."
- Proceed to Phase 3 (Skill Creation)

---

### Phase 2: Storage Confirmation

**Goal**: Determine where to store the skill.

Ask user using AskUserQuestion:
```
Where should this skill be stored?

1. LOCAL (.claude/skills/)
   - Project-specific
   - Shared with team via git

2. GLOBAL (~/.claude/skills/)
   - Personal
   - Available across all projects
```

Remember the choice for later use.

---

### Phase 3: Skill Creation (if not found)

**Goal**: Create a new skill through deep research.

Follow the creation workflow in [references/skill-create.md](references/skill-create.md).

Steps:
1. Research the domain thoroughly using WebSearch
2. Identify best practices and common patterns
3. Design the skill structure
4. Generate SKILL.md following official format
5. Save to the location user chose in Phase 2

---

### Phase 4: Skill Execution

**Goal**: Execute the skill to complete the user's original task.

#### Step 4.1: Load the Skill

Read the skill's SKILL.md:
```
Read: <skill_path>/SKILL.md
```

#### Step 4.2: Execute

Follow the skill's instructions to complete the user's task.

**Important**: Execute naturally. Don't force adherence to the skill if something doesn't fit - adapt as needed. These adaptations become improvement signals.

#### Step 4.3: Complete the Task

Deliver the output to the user.

---

### Phase 5: Skill Review

**Goal**: Determine if the skill needs improvement based on actual execution.

**CRITICAL**: This must happen in a fresh agent context using Task tool.

#### Step 5.1: Spawn Fresh Agent

Use Task tool to create a fresh agent for review:

```
Task: "Review skill execution for improvements"

You are reviewing a skill execution. You have:
1. The SKILL.md content (provided below)
2. The conversation memory of what actually happened (this conversation)

Your job: Compare what the skill SAYS vs what ACTUALLY HAPPENED.

## The Skill
<paste SKILL.md content here>

## Review Questions
1. Did I have to deviate from the skill's instructions? Where?
2. Did I have to improvise something not in the skill? What?
3. Did the user have to clarify something the skill should have covered?
4. Were there errors/retries that better instructions could prevent?

## Output
If NO divergence: "Skill executed perfectly. No improvements needed."
If divergence found: List specific improvements with format:
- Location: <which part of skill>
- Issue: <what happened during execution>
- Suggestion: <concrete change to make>
```

#### Step 5.2: Handle Review Results

**If no improvements needed:**
- Report to user: "Skill worked well, no updates needed."
- Proceed to Phase 6 (Complete)

**If improvements suggested:**
- Present improvements to user
- Ask: "Would you like me to apply these improvements to the skill?"
- If yes: Proceed to Phase 5.3
- If no: Proceed to Phase 6 (Complete)

#### Step 5.3: Apply Improvements

Follow the improvement workflow in [references/skill-improve.md](references/skill-improve.md).

**For local/personal skills**: Apply changes directly using Edit tool.

**For official/external skills**:
- Cannot modify directly
- Generate improvement suggestions as a document
- Offer to create a PR description if it's on GitHub

---

### Phase 6: Complete

Report final status:
```
## Task Complete

- **Task**: <original request>
- **Skill Used**: <skill name>
- **Skill Location**: <path>
- **Improvements Applied**: Yes / No / N/A

<Any relevant notes>
```

---

## State Management

Track workflow state by writing to `.skill-automata-state.json`:

```json
{
  "request": "original user request",
  "state": "SEARCH | CREATE | EXECUTE | REVIEW | COMPLETE",
  "skill_found": true/false,
  "skill_name": "name",
  "skill_path": "path",
  "storage_location": "local | global",
  "improvements_needed": true/false,
  "improvements_applied": true/false
}
```

Update this file as you progress through phases.

---

## Error Handling

| Error | Recovery |
|-------|----------|
| Search fails (network) | Retry once, then proceed to creation |
| Skill creation fails | Report to user, ask for guidance |
| Execution fails | Capture in memory, still do review |
| Review times out | Skip review, complete workflow |

---

## References

- [skill-search.md](references/skill-search.md) - Skill discovery workflow
- [skill-create.md](references/skill-create.md) - Skill creation workflow
- [skill-review.md](references/skill-review.md) - Execution review workflow
- [skill-improve.md](references/skill-improve.md) - Improvement application workflow
