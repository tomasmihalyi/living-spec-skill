# Living Spec Skill for Claude Code

> **Version:** 2.1 (Claude Code Native Tools Edition)
> **Last Updated:** 2026-02-05

A multi-agent orchestration system that transforms how we create and maintain software specifications using AI-DLC (AI-Driven Development Lifecycle) principles.

## Features

- **Multi-Agent Orchestration** - Parallel subagents via Task tool for faster analysis
- **EARS Format** - Structured requirements syntax (Easy Approach to Requirements Syntax)
- **Comprehension Gates** - Preventing skill atrophy with verification questions
- **Tiered Approval System** - Autonomous vs. blocking changes
- **Brownfield Analysis** - Reverse-engineering existing codebases
- **Feature Specs** - Domain specialist agents for implementation
- **Drift Detection** - Track spec-code divergence
- **Role-Based Views** - Developer, Manager, QA, Architect perspectives
- **Template Variants** - Minimal/Standard/Enterprise options

## Installation

Copy the skill directories to your Claude Code skills folder:

```bash
# Clone the repository
git clone https://github.com/tomasmihalyi/living-spec-skill.git

# Copy to Claude Code skills directory
cp -r living-spec-skill/spec ~/.claude/skills/
cp -r living-spec-skill/agents ~/.claude/skills/
cp -r living-spec-skill/steering ~/.claude/skills/
```

## Usage

```bash
/spec                           # Start new or continue existing
/spec <feature description>     # Create spec for specific feature
/spec status                    # View all specs and their phases
/spec drift                     # Check spec-code drift
/spec update <spec-name>        # Update existing spec
/spec view <role>               # Role-based view (developer|manager|qa|architect)
```

## Directory Structure

```
~/.claude/skills/
├── spec/
│   └── SKILL.md                # Main skill definition
├── agents/
│   ├── requirements-analyst.md # Extract FR/NFR in EARS format
│   ├── architecture-reviewer.md # Analyze design patterns
│   ├── risk-assessor.md        # Security, performance, debt
│   ├── database-specialist.md  # Schema, queries, migrations
│   ├── api-specialist.md       # Endpoints, contracts, errors
│   ├── frontend-specialist.md  # Components, state, UX
│   ├── security-specialist.md  # Auth, validation, threats
│   ├── test-specialist.md      # Test strategy, coverage
│   ├── spec-critic.md          # Review alignment, find gaps
│   ├── comprehension-gate.md   # Verify understanding
│   └── spec-updater.md         # Maintain spec documents
└── steering/
    ├── workflow.md             # Core workflow logic
    ├── template.md             # Living Spec template (with variants)
    ├── maintenance.md          # Project steering template
    ├── drift-detection.md      # Drift calculation logic
    ├── traceability.md         # Requirements tracing
    ├── decisions.md            # Decision-making guidance
    ├── ears-template.md        # EARS format for feature specs
    ├── hooks-template.md       # Claude Code hooks integration
    └── views/
        ├── developer.md        # Developer-focused view
        ├── manager.md          # Manager-focused view
        ├── qa.md               # QA-focused view
        └── architect.md        # Architect-focused view
```

## AI-DLC Phases

| Phase | Icon | Purpose | Sections |
|-------|------|---------|----------|
| Planning | 🔵 | WHAT and WHY | Intent, Requirements, Architecture |
| Building | 🟢 | HOW | Implementation, Metrics |
| Operating | 🟡 | RUN and MEASURE | Decision Log, Next Actions |

## Claude Code Native Tools Integration

The skill leverages Claude Code's native tools for better UX:

| Tool | Usage |
|------|-------|
| **AskUserQuestion** | First-time activation, project type, comprehension gates, transitions |
| **Task** | Spawning parallel analysis agents |
| **TaskCreate/TaskUpdate** | Tracking implementation progress |
| **Bash** | Drift detection (file timestamps) |
| **Glob** | Finding new/unmapped files |
| **Read/Write/Edit** | Spec file management |

## Multi-Agent Architecture

```
┌─────────────────────────────────────────────────────────────────┐
│                    /spec ORCHESTRATOR                            │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  PLANNING PHASE (Parallel)           BUILDING PHASE (Parallel)  │
│  ┌─────────────────────────┐         ┌─────────────────────────┐│
│  │ requirements-analyst    │         │ database-specialist     ││
│  │ architecture-reviewer   │         │ api-specialist          ││
│  │ risk-assessor           │         │ frontend-specialist     ││
│  └─────────────────────────┘         │ security-specialist     ││
│                                      │ test-specialist         ││
│                                      └─────────────────────────┘│
│                                                                  │
│  QUALITY GATES                       MAINTENANCE                 │
│  ┌─────────────────────────┐         ┌─────────────────────────┐│
│  │ spec-critic             │         │ spec-updater            ││
│  │ comprehension-gate      │         │ (drift detection)       ││
│  └─────────────────────────┘         └─────────────────────────┘│
└─────────────────────────────────────────────────────────────────┘
```

## Tiered Approval System

| Tier | Type | Examples |
|------|------|----------|
| **Tier 1** | Autonomous | Timestamps, status icons, drift scores |
| **Tier 2** | Async Notification | Component Map additions, tech debt items |
| **Tier 3** | Synchronous Approval | New requirements, architecture changes, phase transitions |

## Template Variants

| Variant | Best For | Complexity |
|---------|----------|------------|
| **Minimal** | MVPs, hackathons, POCs | Intent + 3-5 questions + 3-5 stages |
| **Standard** | Most projects | Full template with all sections |
| **Enterprise** | Compliance, audit trails | Extended tracking sections |

## EARS Format

Feature specs use EARS (Easy Approach to Requirements Syntax):

| Type | Template |
|------|----------|
| Ubiquitous | THE `<system>` SHALL `<response>` |
| Event-Driven | WHEN `<trigger>` THE `<system>` SHALL `<response>` |
| State-Driven | WHILE `<state>` THE `<system>` SHALL `<response>` |
| Unwanted | IF `<condition>` THEN THE `<system>` SHALL `<response>` |
| Optional | WHERE `<feature>` THE `<system>` SHALL `<response>` |

## Changelog

### v2.1 (2026-02-05) - Claude Code Native Tools Edition

| Change | Before | After |
|--------|--------|-------|
| User prompts | Text-based blocking | AskUserQuestion tool |
| Task tracking | TodoWrite | TaskCreate/TaskUpdate/TaskList |
| Agent spawning | Descriptive text | Task tool with explicit parameters |
| Drift detection | Conceptual | Concrete Bash/Glob implementation |
| Templates | Single template | Minimal/Standard/Enterprise variants |
| Hooks | Custom format | Claude Code PostToolCall format |
| Comprehension gates | Text questions | AskUserQuestion with multiple choice |
| Brownfield flow | Blocking questionnaire | Auto-populate, non-blocking verification |

### v2.0 - Multi-Agent Edition

- Initial multi-agent orchestration
- 11 specialized agents
- EARS format for requirements
- Comprehension gates
- Tiered approval system

## Customization

Edit agent prompts in `~/.claude/skills/agents/*.md` to customize behavior:

```bash
# Example: Modify the requirements analyst
nano ~/.claude/skills/agents/requirements-analyst.md
```

## Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Submit a pull request

## License

MIT License - See LICENSE file for details.

---

*Created with Claude Code and the Living Spec skill itself*
