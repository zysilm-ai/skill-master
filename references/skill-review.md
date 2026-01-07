# Skill Review Workflow

This document describes how to review a skill execution and identify improvements.

## Core Principle

**The review is empirical, not theoretical.**

We compare:
1. **What the skill says to do** (SKILL.md instructions)
2. **What actually happened** (conversation memory from execution)

If execution matched the skill perfectly → **No improvements needed.**
If execution diverged from the skill → **Those divergences are improvements.**

---

## Why Fresh Agent?

The review MUST happen in a fresh agent context (via Task tool) because:

1. **Clean perspective**: The fresh agent didn't struggle through execution, so it can objectively compare script vs reality
2. **No bias**: The executing agent might rationalize its adaptations; fresh eyes see gaps clearly
3. **Clear input**: The fresh agent receives the skill AND the memory as distinct inputs to compare

---

## Review Process

### Step 1: Prepare Review Input

Gather two things:

**A. The Skill Content**
Read the SKILL.md that was executed:
```
Read: <skill_path>/SKILL.md
```

**B. The Execution Memory**
This is the conversation history - what actually happened during execution. The fresh agent receives this as context.

### Step 2: Spawn Fresh Agent

Use Task tool:

```
Task tool with prompt:

"You are reviewing a skill execution. Compare the SKILL against what ACTUALLY HAPPENED.

## The Skill (What Should Happen)
<paste full SKILL.md content>

## Your Task
Review the conversation memory (this conversation up to this point) and answer:

1. Did I follow the skill's instructions exactly, or did I have to deviate?
2. Did I improvise anything that wasn't in the skill?
3. Did the user have to clarify something the skill should have covered?
4. Were there errors or retries that better instructions could prevent?
5. Did the output match what the skill said it would produce?

## Your Output

IF everything matched perfectly:
→ 'RESULT: No improvements needed. Skill executed as written.'

IF there were divergences:
→ List each one:
  - WHERE: <which step/section of skill>
  - WHAT HAPPENED: <what I actually did>
  - SUGGESTION: <how to update the skill>
"
```

### Step 3: Interpret Results

**If "No improvements needed":**
- Report to user: "Skill worked perfectly."
- Done.

**If improvements listed:**
- Present to user: "Based on execution, these improvements could help:"
- List each suggestion
- Ask: "Would you like me to apply these?"

---

## What Counts as Divergence?

| Situation | Is it divergence? | Improvement? |
|-----------|-------------------|--------------|
| Followed skill exactly, succeeded | No | None needed |
| Had to figure out something skill didn't explain | Yes | Add explanation |
| User asked clarifying question | Yes | Add that info to skill |
| Tool failed, had to retry differently | Yes | Add error handling |
| Skipped a step that wasn't needed | Maybe | Consider removing step |
| Added extra step for quality | Yes | Add that step |
| Output format differed from skill | Yes | Update output section |

---

## Example Review

### Skill Said:
```
### Step 3: Generate Report
Create a PDF report with sections A, B, C.
```

### What Actually Happened:
- Tried to create PDF but user didn't have LaTeX
- Had to switch to Markdown output
- User was fine with Markdown

### Improvement:
```
- WHERE: Step 3
- WHAT HAPPENED: PDF generation failed, fell back to Markdown
- SUGGESTION: Add "Output Format" option at start - PDF (requires LaTeX) or Markdown
```

---

## Output Format

The review should produce:

```markdown
## Skill Review Results

**Skill**: <name>
**Execution Outcome**: Success / Partial / Failed

### Divergences Found: <count>

#### 1. <Title>
- **Where**: <skill section>
- **What Happened**: <during execution>
- **Suggestion**: <concrete change>

#### 2. <Title>
...

### Recommendation
<Overall assessment - improve or not>
```

Or if no issues:

```markdown
## Skill Review Results

**Skill**: <name>
**Execution Outcome**: Success

### Divergences Found: 0

The skill executed exactly as written. No improvements needed.
```

---

## Key Points

1. **No arbitrary criteria** - Only real execution issues matter
2. **Evidence-based** - Every suggestion ties to something that actually happened
3. **Self-limiting** - Well-written skills stop needing improvements
4. **User decides** - Present suggestions, user chooses whether to apply
