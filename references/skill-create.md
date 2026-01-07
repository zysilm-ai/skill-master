# Skill Creation Workflow

This document describes how to create a new skill when no existing skill is found.

## Overview

When no suitable skill exists, create one through:
1. Deep research on the domain
2. Understanding best practices
3. Designing the skill structure
4. Writing the SKILL.md

---

## Step 1: Deep Research

### 1.1 Understand the Task

Clarify what needs to be accomplished:
- What is the end goal?
- What are the inputs and outputs?
- Who is the target user?

### 1.2 Research the Domain

Use WebSearch to learn about the domain:

```
WebSearch: "<task>" best practices guide
WebSearch: "<task>" professional workflow steps
WebSearch: how to "<task>" step by step
WebSearch: "<task>" common mistakes to avoid
```

### 1.3 Find Existing Patterns

Look for how others approach this task:

```
WebSearch: "<task>" template example
WebSearch: "<task>" checklist professional
```

### 1.4 Synthesize Findings

From research, identify:
- **Core steps**: What are the essential phases?
- **Best practices**: What do experts recommend?
- **Common pitfalls**: What should be avoided?
- **Tools needed**: What capabilities are required?

---

## Step 2: Design the Skill

### 2.1 Choose a Name

Rules:
- Lowercase letters, numbers, hyphens only
- Max 64 characters
- Descriptive of what it does
- Use gerund form: `creating-reports`, `analyzing-data`

Examples:
- `market-research-reports` ✓
- `business-plan-generator` ✓
- `helper` ✗ (too vague)

### 2.2 Write the Description

Must include:
- **What** the skill does
- **When** to use it (trigger keywords)
- Max 1024 characters

Template:
```
<What it does in detail>. <Key capabilities>. Use when <trigger conditions>.
```

Example:
```
Generates comprehensive business plans with executive summary, market analysis, financial projections, and implementation roadmap. Use when creating business plans, startup pitches, investor presentations, or strategic planning documents.
```

### 2.3 Identify Required Tools

List only tools the skill needs:
- `Read` - Read files
- `Write` - Create files
- `Edit` - Modify files
- `WebSearch` - Research
- `WebFetch` - Fetch specific URLs
- `AskUserQuestion` - Get user input
- `TodoWrite` - Track progress

---

## Step 3: Write the SKILL.md

### 3.1 Structure

```yaml
---
name: <skill-name>
description: "<comprehensive description>"
allowed-tools: <Tool1, Tool2, ...>
---

# <Skill Title>

## Overview
<What this skill does and why>

## When to Use
<Activation conditions>

## Workflow

### Step 1: <First Step>
<Instructions>

### Step 2: <Second Step>
<Instructions>

[Continue as needed]

## Output
<What the skill produces>

## Examples

### Example 1: <Use Case>
<Concrete example>

## Notes
<Important considerations>
```

### 3.2 Writing Guidelines

**DO:**
- Be specific and actionable
- Include concrete steps
- Provide examples
- Handle likely errors

**DON'T:**
- Be vague ("process the data")
- Skip steps that seem obvious
- Assume context
- Over-engineer

---

## Step 4: Save the Skill

### 4.1 Determine Location

Based on user's choice:
- **LOCAL**: `.claude/skills/<skill-name>/SKILL.md`
- **GLOBAL**: `~/.claude/skills/<skill-name>/SKILL.md`

### 4.2 Create Directory

```
Create directory: <location>/<skill-name>/
```

### 4.3 Write SKILL.md

```
Write: <location>/<skill-name>/SKILL.md
Content: <generated skill content>
```

### 4.4 Verify

Read the file back to confirm it was saved correctly.

---

## Example: Creating a Business Plan Skill

### Research Phase

```
WebSearch: business plan best practices structure
WebSearch: business plan sections professional
WebSearch: how to write business plan step by step
```

**Findings:**
- Standard sections: Executive Summary, Company Description, Market Analysis, Organization, Product/Service, Marketing, Financials
- Should include SWOT analysis
- Financial projections typically 3-5 years
- Length: 15-30 pages typically

### Design Phase

**Name**: `business-plan-generator`

**Description**: "Generates comprehensive business plans with executive summary, company overview, market analysis, competitive landscape, marketing strategy, financial projections, and implementation timeline. Includes SWOT analysis and risk assessment. Use when creating business plans, startup documentation, investor pitches, or strategic planning."

**Tools**: `Read, Write, Edit, WebSearch, AskUserQuestion, TodoWrite`

### Generated SKILL.md

```yaml
---
name: business-plan-generator
description: "Generates comprehensive business plans..."
allowed-tools: Read, Write, Edit, WebSearch, AskUserQuestion, TodoWrite
---

# Business Plan Generator

## Overview
Creates professional business plans following industry best practices...

## Workflow

### Step 1: Gather Business Information
Ask user for:
- Business name and concept
- Target market
- Revenue model
...

### Step 2: Market Research
Research the industry using WebSearch...

[etc.]
```

---

## Notes

- Research thoroughly before writing
- Keep skills focused (one main purpose)
- Test by mentally walking through the steps
- The skill will be improved after real execution
