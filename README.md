# Living Spec Plugin for Claude Code

> **Version:** 2.3.0
> **Last Updated:** 2026-02-16

Multi-agent orchestration for AI-maintainable specifications using AI-DLC principles.

## Installation

### From Marketplace

```bash
# Add the marketplace
/plugin marketplace add tomasmihalyi/living-spec-skill

# Install the plugin
/plugin install living-spec-skill@living-spec-marketplace
```

### Direct Install

```bash
claude plugin add github:tomasmihalyi/living-spec-skill
```

### Local Install

```bash
git clone https://github.com/tomasmihalyi/living-spec-skill.git
claude plugin add ./living-spec-skill
```

## Usage

```bash
/spec                    # Start new or continue existing
/spec <feature>          # Create spec for specific feature
/spec status             # View all specs and phases
/spec drift              # Check spec-code drift
/spec view <role>        # Role-based view (developer|manager|qa|architect)
```

## Plugin Structure

```
living-spec-skill/
├── .claude-plugin/
│   └── plugin.json              # Plugin manifest
├── commands/
│   └── spec.md                  # /spec slash command
├── agents/                      # 11 specialist agents (auto-discovered)
│   ├── requirements-analyst.md
│   ├── architecture-reviewer.md
│   ├── risk-assessor.md
│   ├── database-specialist.md
│   ├── api-specialist.md
│   ├── frontend-specialist.md
│   ├── security-specialist.md
│   ├── test-specialist.md
│   ├── spec-critic.md
│   ├── comprehension-gate.md
│   └── spec-updater.md
└── skills/
    └── living-spec/
        ├── SKILL.md             # Auto-activating skill
        └── steering/            # Workflow logic and templates
            ├── workflow.md
            ├── template.md
            ├── ears-reference.md
            ├── ears-template.md
            ├── drift-detection.md
            ├── maintenance.md
            ├── decisions.md
            ├── traceability.md
            ├── hooks-template.md
            └── views/
                ├── developer.md
                ├── manager.md
                ├── qa.md
                └── architect.md
```

## Key Features

- **Multi-Agent Orchestration** - 11 specialized agents with parallel execution
- **EARS Format** - Structured, testable requirements
- **Comprehension Gates** - Prevent skill atrophy at phase transitions
- **Tiered Approval** - Autonomous/Notify/Blocking changes
- **Drift Detection** - Track spec-code divergence
- **Template Variants** - Minimal/Standard/Enterprise
- **Role-Based Views** - Developer, Manager, QA, Architect

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

## Project Structure (Generated)

When `/spec` is activated, it creates:

```
project/
├── CLAUDE.md                   # Project instructions + spec context (auto-loaded)
└── .specs/
    ├── 00-project.living.md    # Main living specification
    └── feature-*/              # Feature specs (Option B)
        ├── requirements.md
        ├── design.md
        └── tasks.md
```

## Changelog

### v2.3.0
- Converted to Claude Code plugin format
- Agents auto-discovered and spawnable as `living-spec-skill:<agent-name>`
- Steering files co-located with skill at `skills/living-spec/steering/`
- Portable paths via `${CLAUDE_PLUGIN_ROOT}`
- Added `model` hints to agent frontmatter

### v2.3
- Simplified: Spec context goes directly in `CLAUDE.md` (no separate steering file)
- Added rule: Sub-agents return text only, orchestrator writes files
- Added CLAUDE.md template section

### v2.2
- Consolidated agent spawning
- Created EARS format reference as single source
- Compressed all agent files

## License

MIT
