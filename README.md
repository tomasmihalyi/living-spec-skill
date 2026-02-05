# Living Spec Skill for Claude Code

> **Version:** 2.2 (Optimized)
> **Last Updated:** 2026-02-05

Multi-agent orchestration for AI-maintainable specifications using AI-DLC principles.

## Installation

```bash
git clone https://github.com/tomasmihalyi/living-spec-skill.git
cp -r living-spec-skill/{spec,agents,steering} ~/.claude/skills/
```

## Usage

```bash
/spec                    # Start new or continue existing
/spec <feature>          # Create spec for specific feature
/spec status             # View all specs and phases
/spec drift              # Check spec-code drift
/spec view <role>        # Role-based view (developer|manager|qa|architect)
```

## Structure

```
~/.claude/skills/
├── spec/SKILL.md           # Entry point (133 lines)
├── agents/
│   ├── INDEX.md            # Agent directory & spawning guide
│   └── [11 agents]         # Compressed agent files (~60 lines each)
└── steering/
    ├── workflow.md         # Core workflow logic
    ├── template.md         # Spec templates (minimal/standard)
    ├── ears-reference.md   # EARS format reference
    └── drift-detection.md  # Drift calculation
```

## Key Features

- **Multi-Agent Orchestration** - Parallel analysis via Task tool
- **EARS Format** - Structured, testable requirements
- **Comprehension Gates** - Prevent skill atrophy
- **Tiered Approval** - Autonomous/Notify/Blocking changes
- **Drift Detection** - Track spec-code divergence
- **Template Variants** - Minimal/Standard/Enterprise

## AI-DLC Phases

| Phase | Icon | Purpose |
|-------|------|---------|
| Planning | 🔵 | WHAT and WHY |
| Building | 🟢 | HOW |
| Operating | 🟡 | RUN and MEASURE |

## Agent Directory

| Agent | Purpose | Model |
|-------|---------|-------|
| requirements-analyst | FR/NFR in EARS | sonnet |
| architecture-reviewer | Design patterns | sonnet |
| risk-assessor | Security, debt | sonnet |
| database-specialist | Schema design | haiku |
| api-specialist | REST endpoints | haiku |
| frontend-specialist | Components, UX | haiku |
| security-specialist | Auth, threats | sonnet |
| test-specialist | Test strategy | haiku |
| spec-critic | Gap analysis | haiku |
| comprehension-gate | Understanding | haiku |
| spec-updater | Maintenance | haiku |

## Optimization (v2.2)

| Metric | Before | After | Reduction |
|--------|--------|-------|-----------|
| SKILL.md | 325 lines | 133 lines | 60% |
| Agent files (avg) | 150 lines | 60 lines | 60% |
| Total tokens | ~7,500 | ~4,000 | 47% |

Key changes:
- Consolidated agent spawning into `agents/INDEX.md`
- Created `steering/ears-reference.md` as single source
- Deduplicated SKILL.md to reference steering files
- Compressed all agent files (removed boilerplate examples)

## Claude Code Tools Used

| Tool | Usage |
|------|-------|
| AskUserQuestion | User prompts, comprehension gates |
| Task | Parallel agent spawning |
| TaskCreate/Update | Progress tracking |
| Bash | Drift detection (timestamps) |
| Glob | File discovery |

## License

MIT
