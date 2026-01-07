# Skill Search Workflow

This document describes how to search for existing skills across multiple sources.

## Search Order

Search in this priority order:
1. **Internal/Local Skills** (already installed)
2. **GitHub** (via MCP server)
3. **Web** (broader search)

Stop as soon as a suitable skill is found.

---

## Step 1: Search Internal Skills

### 1.1 Check Global Skills

List skills in user's global directory:

```
Glob: ~/.claude/skills/*/SKILL.md
```

For Windows:
```
Glob: %USERPROFILE%\.claude\skills\*\SKILL.md
```

### 1.2 Check Project Skills

List skills in current project:

```
Glob: .claude/skills/*/SKILL.md
```

### 1.3 Evaluate Local Skills

For each SKILL.md found:
1. Read the file
2. Check if the `description` matches the user's task
3. If match found → Return this skill

---

## Step 2: Search GitHub

### 2.1 Using GitHub MCP Server (Preferred)

If GitHub MCP server is available, use it to search:

```
mcp__github__search_repositories:
  query: "claude code skill SKILL.md <task keywords>"
```

Or search for code:
```
mcp__github__search_code:
  query: "filename:SKILL.md <task keywords>"
```

### 2.2 Using WebSearch (Fallback)

If MCP not available, use WebSearch:

```
WebSearch: site:github.com "claude code" skill SKILL.md <task keywords>
WebSearch: site:github.com "allowed-tools" SKILL.md <domain keywords>
```

### 2.3 Evaluate GitHub Results

For promising repositories:
1. Use WebFetch or MCP to read the repository's SKILL.md
2. Check if description matches user's task
3. Verify it's a valid Claude Code skill (has frontmatter with name, description)
4. Note the repository URL for potential installation

---

## Step 3: Search Web Broadly

If GitHub search yields nothing:

```
WebSearch: "claude code skill" <task keywords>
WebSearch: claude code plugin <domain> <task>
```

Evaluate any results found similarly to GitHub.

---

## Output Format

Return search results in this format:

```markdown
## Search Results

### Source: <internal | github | web>

**Skill Found**: Yes / No

**If Yes:**
- Name: <skill name>
- Location: <path or URL>
- Description: <from SKILL.md>
- Match Confidence: High / Medium / Low

**If No:**
- Sources searched: <list>
- Reason: <why no match>
```

---

## Decision Logic

```
IF internal skill matches task
  → Use internal skill (no installation needed)

ELSE IF GitHub skill matches task
  → Present to user for installation
  → Install to chosen location (local/global)

ELSE IF web skill matches task
  → Present to user with caution (verify source)
  → Install if user approves

ELSE
  → No skill found, proceed to creation
```

---

## Installation

When a skill is found on GitHub:

1. Ask user for storage location (local vs global)

2. Clone or download the skill:
   - For LOCAL: Save to `.claude/skills/<skill-name>/`
   - For GLOBAL: Save to `~/.claude/skills/<skill-name>/`

3. Verify installation:
   - Read the installed SKILL.md
   - Confirm it's valid

---

## Notes

- Prefer internal skills (already vetted)
- GitHub skills from known authors are more trustworthy
- Always verify SKILL.md has valid frontmatter before using
- If multiple skills match, present options to user
