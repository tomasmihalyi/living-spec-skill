# Living Spec Skill for Claude Code

Bring spec-driven development with AI-DLC principles to Claude Code. Equivalent to Kiro's Living Spec Power.

## What It Does

- **Single source of truth** for project intent, requirements, architecture, and progress
- **AI-DLC phases**: 🔵 Planning → 🟢 Building → 🟡 Operating
- **Drift detection** keeps spec and code in sync
- **Traceability** from requirements through design, tasks, and tests
- **Role-based views** for developers, managers, QA, and architects
- **Brownfield support** with codebase reverse engineering

## Installation

### One-liner (Recommended)

```bash
mkdir -p ~/.claude/skills && \
  git clone https://github.com/tomasmihalyi/living-spec-skill.git /tmp/living-spec-skill && \
  cp -r /tmp/living-spec-skill/spec /tmp/living-spec-skill/steering ~/.claude/skills/ && \
  rm -rf /tmp/living-spec-skill
```

### Manual Installation

1. Create the skills directory:
   ```bash
   mkdir -p ~/.claude/skills
   ```

2. Copy the `spec/` directory (contains `SKILL.md`):
   ```bash
   cp -r spec ~/.claude/skills/
   ```

3. Copy the `steering/` directory:
   ```bash
   cp -r steering ~/.claude/skills/
   ```

### Verify Installation

```bash
ls ~/.claude/skills/
# Should show: spec/  steering/

ls ~/.claude/skills/spec/
# Should show: SKILL.md
```

### Restart Claude Code

After installation, restart Claude Code for the skill to be discovered.

## Usage

```bash
# Start living spec workflow (asks for approach A/B/C)
/spec

# Create spec for specific feature
/spec Add user authentication with OAuth

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

## Approaches

On first use, you'll choose an approach:

| Approach | Best For | Creates |
|----------|----------|---------|
| **A) Living Spec Only** | MVPs, small teams, rapid iteration | Single spec file |
| **B) Living Spec + Feature Specs** | Multiple features, growing projects | Orchestrator + feature specs |
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
                    Approval gates between phases
```

## Directory Structure

After setup, your project will have:

```
.specs/
├── maintenance.md              # Always-loaded steering
├── 00-project.living.md        # Main Living Spec
└── feature-auth/               # Feature spec (Option B)
    ├── requirements.md
    ├── design.md
    └── tasks.md
```

## Key Features

### Drift Detection
Automatically tracks when code changes without spec updates:
```
📊 Drift Score: 35% ⚠️

3 files changed since last spec update:
- src/auth/login.ts
- src/auth/logout.ts
- src/api/users.ts

Should I update the Living Spec?
```

### Requirements Questionnaire
Interactive requirements gathering with blocking:
```
⚠️ STOP: Complete the Requirements Questionnaire before proceeding to Architecture.

Q1: Authentication Method
Options: A) JWT B) Session-based C) OAuth only
Your Answer: _______________
Status: ⬚ Unanswered
```

### Traceability Matrix
Track requirements through implementation:
```
| Req ID | Design | Tasks | Tests | Status |
|--------|--------|-------|-------|--------|
| PR-001 | §3.1   | S1,S2 | T-001 | ✅     |
| PR-002 | §3.2   | S3    | -     | ⬚      |
```

### Role-Based Views
Context-appropriate information for each role:
- **Developer**: Next task, component locations, relevant decisions
- **Manager**: Progress, blockers, metrics, pending decisions
- **QA**: Test coverage, acceptance criteria, quality gates
- **Architect**: Design decisions, tech debt, component relationships

## Workflow Example

```
You: /spec Build an e-commerce checkout flow

Claude: Which approach fits your project?
        A) Living Spec Only
        B) Living Spec + Feature Specs
        C) Feature Specs Only

You: A

Claude: Is this a new project or existing codebase?
        A) Greenfield
        B) Brownfield

You: B

Claude: [Analyzes codebase...]

        Codebase Analysis Complete:
        - Architecture: Next.js App Router
        - Database: PostgreSQL with Prisma
        - Existing components: 47 found

        Creating Living Spec...

        [Creates .specs/00-ecommerce.living.md]

        Now in 🔵 Planning phase.

        Let's start with the Requirements Questionnaire:

        Q1: Payment Provider
        Which payment provider will you use?
        A) Stripe
        B) PayPal
        C) Both
        D) Other

        Your answer:
```

## Comparison to Kiro Living Spec Power

| Feature | Kiro | This Skill |
|---------|------|------------|
| AI-DLC phases | ✅ | ✅ |
| Approach selection (A/B/C) | ✅ | ✅ |
| Requirements questionnaire | ✅ | ✅ |
| Drift detection | ✅ | ✅ |
| Traceability matrix | ✅ | ✅ |
| Role-based views | ✅ | ✅ |
| Brownfield reverse engineering | ✅ | ✅ |
| Approval gates | ✅ | ✅ |
| Session continuity | ✅ | ✅ |
| Visual UI | Native IDE | Terminal/Markdown |
| Installation | Built-in | Copy files |

## File Reference

```
living-spec-skill/
├── README.md                        # This file
├── spec/
│   └── SKILL.md                     # Main skill entry point (with YAML frontmatter)
└── steering/
    ├── workflow.md                  # Core workflow logic
    ├── template.md                  # Living Spec template
    ├── maintenance.md               # Maintenance steering template
    ├── drift-detection.md           # Drift detection logic
    ├── traceability.md              # Traceability management
    ├── decisions.md                 # Approach selection guide
    └── views/
        ├── developer.md             # Developer role view
        ├── manager.md               # Manager role view
        ├── qa.md                    # QA role view
        └── architect.md             # Architect role view
```

## Tips

1. **Start every session** with `/spec status` to see where you left off
2. **Run `/spec drift`** before starting new work to ensure spec is current
3. **Use role views** to get context-appropriate information
4. **Don't skip questionnaire** - it ensures requirements are captured
5. **Update spec immediately** after completing tasks to minimize drift

## Troubleshooting

**Skill not loading:**
- Verify `~/.claude/skills/spec/SKILL.md` exists
- Check that YAML frontmatter is at the top of SKILL.md
- Restart Claude Code after installation
- Check file permissions

**Drift score always 0:**
- Ensure Component Map in spec lists your source files
- Timestamps need to be accurate

**Phase gate blocked:**
- Check exit criteria in spec
- All items must be checked before transition

## Updating

To update to the latest version:

```bash
rm -rf ~/.claude/skills/spec ~/.claude/skills/steering && \
  git clone https://github.com/tomasmihalyi/living-spec-skill.git /tmp/living-spec-skill && \
  cp -r /tmp/living-spec-skill/spec /tmp/living-spec-skill/steering ~/.claude/skills/ && \
  rm -rf /tmp/living-spec-skill
```

## License

MIT - Use freely, modify as needed.
