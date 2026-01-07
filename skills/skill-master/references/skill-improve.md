# Skill Improvement Workflow

This document describes how to apply approved improvements to a skill.

## Overview

After review identifies improvements and user approves them, apply changes to the skill. The approach depends on skill ownership:

- **Local/Personal skills**: Edit directly
- **Official/External skills**: Generate PR description (cannot edit)

---

## Step 1: Determine Skill Type

Check where the skill is located:

| Location | Type | Can Edit? |
|----------|------|-----------|
| `.claude/skills/` | Project/Local | Yes |
| `~/.claude/skills/` | Personal/Global | Yes |
| External repo / Official | Third-party | No |

---

## Step 2: Plan Changes

For each approved improvement:

1. **Identify the location** in SKILL.md to change
2. **Determine the change type**:
   - Add new content
   - Modify existing content
   - Remove content
3. **Draft the new content**

---

## Step 3: Apply Changes (Editable Skills)

### 3.1 Read Current Content

```
Read: <skill_path>/SKILL.md
```

### 3.2 Apply Each Improvement

Use Edit tool for surgical changes:

**Adding new content:**
```
Edit: <skill_path>/SKILL.md
old_string: |
  ### Step 3: Process Data
new_string: |
  ### Step 2.5: Validate Input

  Before processing, verify:
  - Input file exists
  - Format is correct

  ### Step 3: Process Data
```

**Modifying content:**
```
Edit: <skill_path>/SKILL.md
old_string: |
  Create a PDF report.
new_string: |
  Create a report in user's preferred format:
  - PDF (requires LaTeX installed)
  - Markdown (no dependencies)

  Ask user which format they prefer.
```

**Improving description:**
```
Edit: <skill_path>/SKILL.md
old_string: |
  description: "Creates reports"
new_string: |
  description: "Creates detailed reports in PDF or Markdown format with data visualization and executive summary. Use when generating reports, creating documentation, or summarizing analysis results."
```

### 3.3 Verify Changes

Read the file again to confirm changes were applied correctly:
```
Read: <skill_path>/SKILL.md
```

---

## Step 4: Handle Non-Editable Skills

For official or external skills that can't be directly edited:

### 4.1 Generate Improvement Document

Create a document with the suggested changes:

```markdown
# Suggested Improvements for <skill-name>

## Source
<repository URL or source>

## Improvements

### 1. <Improvement Title>

**Location**: <section of SKILL.md>

**Current**:
```
<current content>
```

**Suggested**:
```
<improved content>
```

**Reason**: <why this helps, based on execution>

### 2. <Next Improvement>
...

## How to Apply

If you maintain this skill:
1. Review the suggestions above
2. Apply changes that make sense
3. Test the updated skill

If contributing to open source:
1. Fork the repository
2. Apply the changes
3. Submit a pull request with this description
```

### 4.2 Present to User

```
I cannot directly edit this skill because it's from <source>.

I've created an improvement document with the suggested changes.
You can:
1. Apply these changes yourself if you have access
2. Submit a PR to the skill's repository
3. Create a local copy and apply changes there

Would you like me to save this document?
```

---

## Step 5: Report Results

After applying improvements:

```markdown
## Improvements Applied

**Skill**: <name>
**Location**: <path>

### Changes Made

1. **<Improvement>**
   - Section: <where>
   - Change: <what was changed>

2. **<Improvement>**
   ...

### Verification
- [x] Changes applied successfully
- [x] SKILL.md still valid
- [x] No syntax errors

The skill has been updated and will use these improvements on next execution.
```

---

## Common Improvement Patterns

### Adding Input Validation

```markdown
### Step 0: Validate Input

Before proceeding, verify:
- [ ] Required input exists
- [ ] Format is as expected
- [ ] Permissions are sufficient

**If validation fails**: Report specific issue and stop.
```

### Adding Format Options

```markdown
### Output Format

Ask user for preferred format:
- **PDF**: Professional, requires LaTeX
- **Markdown**: Universal, no dependencies
- **HTML**: Web-ready, includes styling
```

### Adding Error Recovery

```markdown
**If <operation> fails:**
1. Check: <common cause>
2. Try: <alternative approach>
3. Ask user if still stuck
```

### Clarifying Ambiguous Steps

```markdown
### Step 3: Process Data

**Specifically:**
1. Read input file as CSV with UTF-8 encoding
2. Parse headers from first row
3. Validate all required columns exist: name, date, value
4. Convert date strings to ISO format
5. Output to JSON
```

---

## Notes

- Keep changes minimal and focused
- Each change should trace back to a real execution issue
- Verify the skill still works after changes
- Don't over-engineer - only add what execution showed was needed
