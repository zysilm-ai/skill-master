# Skill Search Workflow

## Search Order - MUST Follow in Sequence

1. MUST search internal/local skills first
2. MUST search ALL known GitHub repos before broader search
3. MUST search web only after GitHub exhausted

## Continue vs Stop Rules

**STOP searching when**: A skill with matching description is found AND verified with valid frontmatter

**MUST CONTINUE when**:
- A search returns no results → proceed to next search in current step
- Current step incomplete → finish ALL searches in step before moving to next step

**NEVER skip**: Known repository searches - MUST complete ALL before broader search

## Step 1: Search Internal Skills

```
Glob: ~/.claude/skills/*/SKILL.md
Glob: .claude/skills/*/SKILL.md
```

For each SKILL.md found:
1. Read the file
2. Check if `description` matches the user's task
3. If match → use this skill

## Step 2: Search GitHub

### 2.1 Search Known Repositories

**MUST complete ALL searches before proceeding to 2.2.**

Copy and track progress:
```
Known Repo Search Progress:
- [ ] anthropics/skills
- [ ] K-Dense-AI/claude-scientific-skills
- [ ] ComposioHQ/awesome-claude-skills
```

Execute each search in order:
1. `WebSearch: site:github.com/anthropics/skills SKILL.md <keywords>`
2. `WebSearch: site:github.com/K-Dense-AI/claude-scientific-skills SKILL.md <keywords>`
3. `WebSearch: site:github.com/ComposioHQ/awesome-claude-skills SKILL.md <keywords>`

**Validation gate**: Before proceeding to Step 2.2, verify:
- [ ] All three searches executed
- [ ] Results documented (found/not found for each)

If ANY search was skipped, STOP and complete it before continuing.

### 2.2 Enumerate Repository Contents (Required Fallback)

**MUST enumerate ALL known repos if WebSearch returned no matches.**

For EACH repository in [known-skill-repos.md](known-skill-repos.md):

```
WebFetch:
  url: <API Enumeration URL from known-skill-repos.md>
  prompt: "List all directory names"
```

Copy and track progress:
```
Enumeration Progress:
- [ ] anthropics/skills enumerated
- [ ] K-Dense-AI/claude-scientific-skills enumerated
- [ ] ComposioHQ/awesome-claude-skills enumerated
```

Scan enumerated names for keyword matches against user's request.

**Validation gate**: MUST complete BOTH WebSearch (2.1) AND enumeration (2.2) before concluding "no skill found in known repos."

### 2.3 Broader GitHub Search

**Only proceed here after Steps 2.1 AND 2.2 completed with no match.**

```
WebSearch: site:github.com SKILL.md "allowed-tools" <task keywords>
```

### 2.4 Retrieve SKILL.md Content

Convert blob URL to raw URL:
```
github.com/{owner}/{repo}/blob/{branch}/{path}/SKILL.md
→ raw.githubusercontent.com/{owner}/{repo}/{branch}/{path}/SKILL.md
```

Fetch content:
```
WebFetch:
  url: https://raw.githubusercontent.com/<owner>/<repo>/main/<path>/SKILL.md
  prompt: "Return the complete raw content exactly as-is"
```

Verify response contains:
- YAML frontmatter (`---`)
- Fields: `name:`, `description:`, `allowed-tools:`

If WebFetch returns a summary instead of raw content:
```bash
curl -sf https://raw.githubusercontent.com/<owner>/<repo>/main/<path>/SKILL.md
```

### 2.5 Evaluate Results

For each skill found:
1. Verify valid frontmatter
2. Check description matches user's task
3. Note repository URL for installation

## Step 3: Search Web

```
WebSearch: SKILL.md "allowed-tools" <task keywords> -site:github.com
```

Fetch and verify any skill found.
