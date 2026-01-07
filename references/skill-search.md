# Skill Search Workflow

This document describes how to search for existing skills across multiple sources.

---

## Search Order

Search in this priority order:
1. **Internal/Local Skills** (already installed)
2. **GitHub via WebSearch + WebFetch** (multi-stage, tiered search)
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

## Step 2: Search GitHub (WebSearch + WebFetch)

### Why WebSearch + WebFetch?

WebSearch alone returns summaries, which lose skill details needed for matching.
The solution: use WebSearch to **discover** skills, then WebFetch with raw GitHub URLs to **retrieve full content**.

### URL Transformation

GitHub blob URLs must be converted to raw URLs for content retrieval:

```
github.com/{owner}/{repo}/blob/{branch}/{path}/SKILL.md
                    ↓
raw.githubusercontent.com/{owner}/{repo}/{branch}/{path}/SKILL.md
```

### 2.1 Stage 1: Search Known Skill Collections

Search high-quality, curated repositories first.
See [known-skill-repos.md](known-skill-repos.md) for the full list.

**Tier 1 (Official/Verified):**

```
WebSearch: site:github.com/anthropics/skills SKILL.md <keywords>

WebSearch: site:github.com/anthropics/claude-code-skills SKILL.md <keywords>

WebSearch: site:github.com/K-Dense-AI/claude-scientific-skills SKILL.md <keywords>

WebSearch: site:github.com/ComposioHQ/awesome-claude-skills SKILL.md <keywords>
```

### 2.2 Stage 2: Broader GitHub Search

If no match in known repos, search all of GitHub:

```
WebSearch: site:github.com SKILL.md "allowed-tools" <task keywords>

WebSearch: site:github.com SKILL.md "description:" <domain keywords>
```

### 2.3 Stage 3: Retrieve Full SKILL.md Content

Once a promising skill is found from search results, extract the URL and convert to raw format:

**Step 1: Parse search result URL**
```
Found: github.com/owner/repo/blob/main/skills/market-research/SKILL.md
```

**Step 2: Convert to raw URL**
```
https://raw.githubusercontent.com/owner/repo/main/skills/market-research/SKILL.md
```

**Step 3: Fetch raw content**
```
WebFetch:
  url: https://raw.githubusercontent.com/owner/repo/main/skills/market-research/SKILL.md
  prompt: "Return the complete raw content of this SKILL.md file exactly as-is"
```

**Alternative: Use Bash with curl** (if WebFetch has issues)
```bash
curl -sf https://raw.githubusercontent.com/<owner>/<repo>/main/<path>/SKILL.md || \
curl -s https://raw.githubusercontent.com/<owner>/<repo>/master/<path>/SKILL.md
```

### 2.4 Evaluate GitHub Results

For each skill found:
1. Retrieve full SKILL.md content via WebFetch or curl
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

**Note**: For any skill found, attempt to get raw content via WebFetch.

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
   # Use curl for each file
   ```

3. Verify installation:
   - Read the installed SKILL.md
   - Confirm it's valid

4. Create source.md for provenance tracking (see SKILL.md Phase 2)

---

## Notes

- WebSearch finds skills, WebFetch retrieves full content
- Always convert GitHub blob URLs to raw.githubusercontent.com URLs
- Prefer Tier 1 repos (anthropics/skills, K-Dense-AI) for quality
- Always verify SKILL.md has valid frontmatter before using
- If multiple skills match, present options to user ranked by:
  1. Match confidence
  2. Repository tier (Tier 1 > Tier 2 > unranked)
  3. License permissiveness
