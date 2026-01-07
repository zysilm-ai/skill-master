# Known Skill Repositories

Curated list of high-quality skill sources for prioritized searching.

## Tier 1: Official & Verified

These repositories are maintained by known organizations with quality standards.

| Repository | Description | Search Priority |
|------------|-------------|-----------------|
| `anthropics/skills` | Official Anthropic skills repository | 1 |
| `K-Dense-AI/claude-scientific-skills` | 139 scientific skills (MIT licensed) | 1 |
| `ComposioHQ/awesome-claude-skills` | Curated awesome list of Claude skills | 1 |

## Tier 2: Specialized Collections

Domain-specific skill repositories.

| Repository | Description | Domain |
|------------|-------------|--------|
| `levnikolaevich/claude-code-skills` | Agile automation (52 skills) | DevOps |
| `meetrais/claude-agent-skills` | Agent skill examples | Development |

---

## WebSearch Patterns

### Stage 1: Search Tier 1 repos first
```
WebSearch: site:github.com/anthropics/skills SKILL.md <keywords>

WebSearch: site:github.com/K-Dense-AI/claude-scientific-skills SKILL.md <keywords>

WebSearch: site:github.com/ComposioHQ/awesome-claude-skills SKILL.md <keywords>
```

### Stage 2: Broader GitHub search
```
WebSearch: site:github.com SKILL.md "allowed-tools" <keywords>
```

### Stage 3: Fetch raw content
Once a skill URL is found, convert to raw URL and fetch:
```
# Convert: github.com/owner/repo/blob/main/path/SKILL.md
# To:      raw.githubusercontent.com/owner/repo/main/path/SKILL.md

WebFetch:
  url: https://raw.githubusercontent.com/<owner>/<repo>/main/<path>/SKILL.md
  prompt: "Return the complete raw content of this SKILL.md file exactly as-is"
```

---

## Adding New Repositories

When you discover a quality skill repository:

1. Verify it contains valid SKILL.md files (proper frontmatter)
2. Check license compatibility
3. Assess quality (documentation, maintenance)
4. Add to appropriate tier in this file

---

## Notes

- Always search Tier 1 before Tier 2
- Prefer repos with explicit licenses
- Check `source.md` if present for provenance info
- Repository quality > repository size
