# Living Spec Skill for Claude Code

> **v2.0 - Multi-Agent Architecture**

Bring spec-driven development with AI-DLC principles to Claude Code. Features **11 specialized subagents** for parallel analysis and domain expertise.

## What It Does

- **Multi-Agent Orchestration** - Parallel subagents for 10x faster analysis
- **Single source of truth** for project intent, requirements, architecture, and progress
- **AI-DLC phases**: 🔵 Planning → 🟢 Building → 🟡 Operating
- **Comprehension Gates** - Prevent skill atrophy with verification questions
- **EARS Format** - Structured, testable requirements syntax
- **Drift detection** keeps spec and code in sync
- **Tiered Approval System** - Right gates at the right places
- **Role-based views** for developers, managers, QA, and architects
- **Brownfield support** with parallel codebase reverse engineering

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
│  │ comprehension-gate      │         │ (autonomous updates)    ││
│  └─────────────────────────┘         └─────────────────────────┘│
└─────────────────────────────────────────────────────────────────┘
```

### 11 Specialized Agents

| Agent | Purpose | Phase |
|-------|---------|-------|
| `requirements-analyst` | Extract FR/NFR in EARS format | Planning |
| `architecture-reviewer` | Analyze design patterns | Planning |
| `risk-assessor` | Security, performance, debt | Planning |
| `database-specialist` | Schema, queries, migrations | Building |
| `api-specialist` | Endpoints, contracts, errors | Building |
| `frontend-specialist` | Components, state, UX | Building |
| `security-specialist` | Auth, validation, threats | Building |
| `test-specialist` | Test strategy, coverage | Building |
| `spec-critic` | Review alignment, find gaps | All |
| `comprehension-gate` | Verify developer understanding | Transitions |
| `spec-updater` | Maintain spec documents | Continuous |

## Installation

### One-liner (Recommended)

```bash
mkdir -p ~/.claude/skills && \
  git clone https://github.com/tomasmihalyi/living-spec-skill.git /tmp/living-spec-skill && \
  cp -r /tmp/living-spec-skill/spec /tmp/living-spec-skill/steering /tmp/living-spec-skill/agents ~/.claude/skills/ && \
  rm -rf /tmp/living-spec-skill
```

### Manual Installation

1. Create the skills directory:
   ```bash
   mkdir -p ~/.claude/skills
   ```

2. Copy all directories:
   ```bash
   cp -r spec steering agents ~/.claude/skills/
   ```

### Verify Installation

```bash
ls ~/.claude/skills/
# Should show: agents/  spec/  steering/

ls ~/.claude/skills/agents/
# Should show 11 agent .md files
```

### Restart Claude Code

After installation, restart Claude Code for the skill to be discovered.

## Usage

```bash
# Start living spec workflow
/spec

# Check project status
/spec status

# Check spec-code drift
/spec drift

# Update existing spec
/spec update

# Role-based views
/spec view developer    # What should I work on?
/spec view manager      # Project status summary
/spec view qa           # Test coverage & gaps
/spec view architect    # Design decisions & tech debt
```

## Key Features

### Parallel Agent Analysis

When analyzing a brownfield codebase, three agents work simultaneously:

```
Spawning parallel agents...

┌─────────────────┐  ┌─────────────────┐  ┌─────────────────┐
│  requirements-  │  │  architecture-  │  │  risk-          │
│  analyst        │  │  reviewer       │  │  assessor       │
│  (FR/NFR)       │  │  (patterns)     │  │  (threats)      │
└─────────────────┘  └─────────────────┘  └─────────────────┘

Results in ~30 seconds:
- 23 Functional Requirements (EARS format)
- 13 Non-Functional Requirements
- 5 Security Risks identified
- 6 Technical Debt items
```

### EARS Requirements Format

All requirements use EARS (Easy Approach to Requirements Syntax):

| Type | Template | Example |
|------|----------|---------|
| Ubiquitous | THE system SHALL [behavior] | THE system SHALL validate all input |
| Event-Driven | WHEN [trigger] THE system SHALL [response] | WHEN user submits form THE system SHALL save data |
| State-Driven | WHILE [state] THE system SHALL [behavior] | WHILE offline THE system SHALL queue requests |
| Unwanted | IF [condition] THEN THE system SHALL [response] | IF database unavailable THEN return 503 |

### Comprehension Gates

Prevent skill atrophy at phase transitions:

```
📋 COMPREHENSION VERIFICATION

Before proceeding to Building, please answer:

1. Why did we choose DynamoDB over PostgreSQL for this architecture?
2. If a JWT token is stolen, what's the impact and mitigation?
3. What is a Lambda cold start and how might it affect UX?

Your responses will be logged in §6 Decision Log.
```

**Why?** Research shows AI assistance can reduce skill mastery by 17% when developers rubber-stamp without understanding.

### Tiered Approval System

Not all changes need human approval:

| Tier | Changes | Behavior |
|------|---------|----------|
| **Tier 1: Autonomous** | Timestamps, status icons, drift scores | Auto-updated |
| **Tier 2: Async Notification** | Component Map, Tech Debt, Next Actions | Update + notify |
| **Tier 3: Synchronous Approval** | Requirements, Architecture, Phase transitions | Blocks until approved |

### Domain Specialist Network

When creating feature specs, relevant specialists spawn in parallel:

```
Creating feature spec for DynamoDB migration...

┌──────────┐ ┌──────────┐ ┌──────────┐ ┌──────────┐
│ database │ │ api      │ │ security │ │ test     │
│ specialist│ │ specialist│ │ specialist│ │ specialist│
└──────────┘ └──────────┘ └──────────┘ └──────────┘

Outputs:
├── requirements.md  ← 18 FR + 13 NFR
├── design.md        ← Schema, API changes, CloudFormation
└── tasks.md         ← 15 tasks, 10 test cases
```

### Drift Detection

```
📊 Drift Score: 35% ⚠️

3 files changed since last spec update:
- src/auth/login.ts
- src/auth/logout.ts
- src/api/users.ts

Should I update the Living Spec?
```

| Score | Status | Action |
|-------|--------|--------|
| 0-20% | ✅ Healthy | Continue working |
| 21-50% | ⚠️ Review | Suggest sync |
| 51%+ | 🔴 Critical | Block until synced |

## Approaches

On first use, choose an approach:

| Approach | Best For | Creates |
|----------|----------|---------|
| **A) Living Spec Only** | MVPs, small teams, rapid iteration | Single spec file |
| **B) Living Spec + Feature Specs** | Multiple features, growing projects | Orchestrator + EARS feature specs |
| **C) Feature Specs Only** | Simple, isolated features | Individual specs |

## AI-DLC Phases

```
🔵 PLANNING          🟢 BUILDING           🟡 OPERATING
─────────────────    ─────────────────    ─────────────────
What & Why           How                   Run & Measure

• Intent             • Execution Plan      • Deployment
• Requirements       • Component Map       • Metrics
• Architecture       • Tech Debt           • Decision Log
                     • Metrics Setup       • Next Actions

     ────────────────────────────────────────────────────→
         Approval gates + Comprehension verification
```

## Directory Structure

After setup, your project will have:

```
project/
├── .claude/
│   └── spec-steering.md              # Auto-loaded every session
├── .specs/
│   ├── 00-project.living.md          # Main Living Spec
│   └── feature-auth/                 # Feature spec (Option B)
│       ├── requirements.md           # EARS format
│       ├── design.md                 # Architecture decisions
│       └── tasks.md                  # Implementation tasks
└── CLAUDE.md                         # Project instructions
```

## Workflow Example

```
You: /spec

Claude: Which approach fits your project?
        A) Living Spec Only
        B) Living Spec + Feature Specs
        C) Feature Specs Only

You: B

Claude: Is this greenfield or brownfield?

You: B (brownfield)

Claude: Spawning parallel analysis agents...

        ┌─────────────────┐ ┌─────────────────┐ ┌─────────────────┐
        │ requirements    │ │ architecture    │ │ risk            │
        │ analyst         │ │ reviewer        │ │ assessor        │
        └─────────────────┘ └─────────────────┘ └─────────────────┘

        Analysis Complete:
        - Architecture: React + Express + SQLite
        - Requirements: 23 FR, 13 NFR extracted
        - Risks: 5 security issues identified

        Creating Living Spec...

        Now in 🔵 Planning phase.

        Requirements Questionnaire:

        Q1: Authentication Strategy
        A) No auth  B) Basic auth  C) OAuth  D) Defer

You: B

Claude: Q2: Database
        A) Keep SQLite  B) PostgreSQL  C) DynamoDB

You: C

Claude: Architecture decisions proposed:
        - D6: DynamoDB (serverless)
        - D7: JWT authentication
        - D8: Lambda deployment

        📋 COMPREHENSION VERIFICATION

        Before approving, please answer:
        1. Why DynamoDB over PostgreSQL?
        2. What if a JWT token is stolen?

You: [answers questions]

Claude: ✅ Approved. Entering 🟢 Building phase.

        Ready to create feature specs with domain specialists.
```

## File Reference

```
living-spec-skill/
├── README.md
├── spec/
│   └── SKILL.md                      # Main skill (with YAML frontmatter)
├── agents/                           # 11 specialized subagents
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
└── steering/                         # Workflow steering files
    ├── workflow.md                   # Core workflow + parallel orchestration
    ├── template.md                   # Living Spec template
    ├── maintenance.md                # Maintenance + tiered approvals
    ├── drift-detection.md            # Drift detection logic
    ├── traceability.md               # Traceability management
    ├── decisions.md                  # Approach selection
    ├── ears-template.md              # EARS feature spec template
    ├── hooks-template.md             # Claude Code hooks config
    └── views/
        ├── developer.md
        ├── manager.md
        ├── qa.md
        └── architect.md
```

## Customization

### Modify Agents

Edit agent prompts in `~/.claude/skills/agents/*.md`:

```markdown
---
name: my-custom-specialist
description: Use this agent when...
color: cyan
tools: ["Read", "Grep", "Glob"]
---

You are a specialist in [domain]...
```

### Add New Agents

Create new `.md` files in `~/.claude/skills/agents/` following the same format.

### Configure Hooks

Copy `steering/hooks-template.md` to your project's `.claude/hooks/hooks.json` for automated drift detection.

## Tips

1. **Start every session** with `/spec status` to see where you left off
2. **Run `/spec drift`** before starting new work
3. **Answer comprehension questions thoughtfully** - they ensure you understand the system
4. **Use domain specialists** for feature specs - they provide expert-level analysis
5. **Update spec immediately** after completing tasks to minimize drift

## Troubleshooting

**Skill not loading:**
- Verify `~/.claude/skills/spec/SKILL.md` exists
- Check that agents directory exists: `ls ~/.claude/skills/agents/`
- Restart Claude Code after installation

**Agents not spawning:**
- Ensure Task tool is available in your Claude Code version
- Check agent files have valid YAML frontmatter

**Comprehension gate blocking:**
- Answer the questions to proceed
- Responses are logged in Decision Log for audit

## Updating

To update to the latest version:

```bash
rm -rf ~/.claude/skills/spec ~/.claude/skills/steering ~/.claude/skills/agents && \
  git clone https://github.com/tomasmihalyi/living-spec-skill.git /tmp/living-spec-skill && \
  cp -r /tmp/living-spec-skill/spec /tmp/living-spec-skill/steering /tmp/living-spec-skill/agents ~/.claude/skills/ && \
  rm -rf /tmp/living-spec-skill
```

## Version History

| Version | Changes |
|---------|---------|
| v2.0 | Multi-agent architecture, 11 specialists, comprehension gates, tiered approvals |
| v1.0 | Initial release with AI-DLC phases, drift detection, role views |

## License

MIT - Use freely, modify as needed.
