# SearXNG Skills TDD Experiment

## Goal
Perfect the searxng-* skills using TDD for documentation.

---

## RED Phase Results

### Scenario 1: Discovery Test
**Result:** ❌ FAILURE
- Agent used `tavily-search` skill (paid service)
- Did NOT discover or use `searxng-search` skill

### Scenario 2: Free Preference Test
**Result:** ⚠️ PARTIAL FAILURE
- Agent correctly suggested SearXNG conceptually
- But did NOT use the `searxng-cli` skill for concrete instructions

### Scenario 4: Research Request Test
**Result:** ✅ CORRECT
- Agent correctly used `tavily-research` skill

---

## Failures Identified

| Failure | Root Cause | Fix Applied |
|---------|------------|-------------|
| Skill not discovered | Description lacked "free" triggers | Added "free", "no API key", "local", "private" triggers |
| Skill not invoked | No connection between "free search" and skill | Added explicit trigger phrases |
| No fallback guidance | Missing troubleshooting | Added "Prerequisites Check" section |

---

## GREEN Phase: Skill Updates

### searxng-search
- Added triggers: "free", "no API key", "local", "private", "self-hosted", "without paying"
- Added "Prerequisites Check" section
- Added "When NOT to use" section

### searxng-cli
- Added triggers: "free alternative to Tavily", "self-hosted search", "local search API"
- Added "Prerequisites Check" section
- Added "Limitations" section

### searxng-extract
- Added triggers: "free", "no API key", "local", "private"
- Added "Prerequisites Check" section
- Added "When NOT to use" section

---

## REFACTOR Phase: Verification

### Discovery Issue Found
Skills are properly formatted and in correct location (`~/.agents/skills/`), but:
- System caches skills list at session start
- New skills won't appear until next session
- This is expected behavior, not a skill defect

### Verification Status
- ✅ Skills properly formatted (YAML frontmatter, name, description)
- ✅ Skills in correct location
- ✅ Skills follow tavily-* patterns
- ⏳ Full verification requires new session

---

## Final Skill Structure

```
~/.agents/skills/
├── searxng-cli/SKILL.md      # Overview, workflow, endpoints
├── searxng-search/SKILL.md   # Web search details
└── searxng-extract/SKILL.md  # URL extraction details
```

---

## Lessons Learned

1. **Description triggers are critical** - Must include all keywords users might say
2. **"When NOT to use" is essential** - Prevents misuse
3. **Prerequisites check** - Helps agents verify environment before use
4. **Session caching** - New skills require session restart to appear

---

## Skill Quality Checklist

- [x] YAML frontmatter with name, description
- [x] Description starts with "Use when..."
- [x] Rich trigger keywords for discovery
- [x] Prerequisites section
- [x] "When to use" section
- [x] "When NOT to use" section
- [x] Quick start examples
- [x] Options/Response tables
- [x] Tips section
- [x] "See also" cross-references
- [x] Word count < 500
- [x] Follows tavily-* patterns
