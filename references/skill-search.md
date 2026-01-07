# Skill Search Workflow

This document describes how to search for existing skills across multiple sources.

## Prerequisites

**GitHub MCP Required**: This workflow requires GitHub MCP for reliable skill discovery.
See Phase 0 in SKILL.md for setup instructions.

---

## Search Order

Search in this priority order:
1. **Internal/Local Skills** (already installed)
2. **GitHub via MCP** (multi-stage, tiered search)
3. **Web** (non-GitHub sources only)

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

## Step 2: Search GitHub (MCP Only)

**Important**: Use GitHub MCP exclusively. Do NOT use WebSearch for GitHub.
WebSearch returns summaries, not raw content - unusable for skill retrieval.

### 2.1 Stage 1: Search Known Skill Collections

Search high-quality, curated repositories first.
See [known-skill-repos.md](known-skill-repos.md) for the full list.

**Tier 1 (Official/Verified):**
```
mcp__github__search_code:
  query: "filename:SKILL.md repo:anthropics/skills <keywords>"

mcp__github__search_code:
  query: "filename:SKILL.md repo:K-Dense-AI/claude-scientific-skills <keywords>"

mcp__github__search_code:
  query: "filename:SKILL.md repo:ComposioHQ/awesome-claude-skills <keywords>"
```

### 2.2 Stage 2: Broader GitHub Search

If no match in known repos, search all of GitHub:

```
mcp__github__search_code:
  query: "filename:SKILL.md <task keywords>"

mcp__github__search_code:
  query: "filename:SKILL.md allowed-tools <domain keywords>"
```

### 2.3 Stage 3: Retrieve Full SKILL.md Content

Once a promising skill is found, get the full content:

**Primary: GitHub MCP**
```
mcp__github__get_file_contents:
  owner: <owner>
  repo: <repo>
  path: <path/to/SKILL.md>
```

**Fallback: Bash with curl** (if MCP get_file_contents fails)
```bash
curl -sf https://raw.githubusercontent.com/<owner>/<repo>/main/<path>/SKILL.md || \
curl -s https://raw.githubusercontent.com/<owner>/<repo>/master/<path>/SKILL.md
```

### 2.4 Evaluate GitHub Results

For each skill found:
1. Retrieve full SKILL.md content (use MCP or curl, NOT WebFetch)
2. Verify valid frontmatter (name, description, allowed-tools)
3. Check if description matches user's task
4. Assess match confidence: High / Medium / Low
5. Note repository URL for installation

---

## Step 3: Search Web (Non-GitHub Only)

Use WebSearch only for non-GitHub sources (blogs, docs, other platforms):

```
WebSearch: SKILL.md "allowed-tools" <task keywords> -site:github.com
```

**Note**: WebSearch is unreliable for skill content retrieval.
Use only to discover sources, then fetch content via other means.

---

## Output Format

Return search results in this format:

```markdown
## Search Results

### Source: <internal | github | web>

**Skill Found**: Yes / No

**If Yes:**
- Name: <skill name>
- Repository: <owner/repo>
- Path: <path/to/SKILL.md>
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

2. Download the skill directory:
   ```bash
   # Create skill directory
   mkdir -p .claude/skills/<skill-name>

   # Download SKILL.md
   curl -s https://raw.githubusercontent.com/<owner>/<repo>/main/<path>/SKILL.md \
     -o .claude/skills/<skill-name>/SKILL.md

   # Download any additional files (references/, assets/, etc.)
   # Use mcp__github__get_file_contents or curl for each file
   ```

3. Verify installation:
   - Read the installed SKILL.md
   - Confirm it's valid

4. Create source.md for provenance tracking (see SKILL.md Phase 2)

---

## Notes

- GitHub MCP is required - do not fall back to WebSearch for GitHub
- Prefer Tier 1 repos (anthropics/skills, K-Dense-AI) for quality
- Always verify SKILL.md has valid frontmatter before using
- If multiple skills match, present options to user ranked by:
  1. Match confidence
  2. Repository tier (Tier 1 > Tier 2 > unranked)
  3. License permissiveness
