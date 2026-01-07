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

## MCP Search Patterns

### Stage 1: Search Tier 1 repos first
```
mcp__github__search_code:
  query: "filename:SKILL.md repo:anthropics/skills <keywords>"

mcp__github__search_code:
  query: "filename:SKILL.md repo:K-Dense-AI/claude-scientific-skills <keywords>"

mcp__github__search_code:
  query: "filename:SKILL.md repo:ComposioHQ/awesome-claude-skills <keywords>"
```

### Stage 2: Broader GitHub search
```
mcp__github__search_code:
  query: "filename:SKILL.md <keywords>"
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
