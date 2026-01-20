# /spec - Living Specifications for Claude Code

Consolidate development artifacts into single, AI-maintainable specification files with AI-DLC principles.

## Usage

```
/spec                           # Start new or continue existing
/spec <feature description>     # Create spec for specific feature
/spec status                    # View all specs and their phases
/spec drift                     # Check spec-code drift
/spec update <spec-name>        # Update existing spec
/spec view <role>               # Role-based view (developer|manager|qa|architect)
```

## First-Time Activation

**MANDATORY: On first trigger in a project, you MUST ask the user which approach before doing anything else.**

Present these options:

```
Which approach fits your project?

A) LIVING SPEC ONLY
   Best for: MVPs, small teams (1-5), rapid iteration
   Creates: Single Living Spec + maintenance steering

B) LIVING SPEC + FEATURE SPECS
   Best for: Multiple features, growing projects
   Creates: Living Spec orchestrates individual feature specs

C) FEATURE SPECS ONLY
   Best for: Clear feature boundaries, simple projects
   Creates: Individual specs per feature (basic workflow)

Your choice (A/B/C):
```

**Do NOT proceed until user responds with A, B, or C.**

## Steering Files

Load these contextually based on workflow phase:

| File | When to Load |
|------|--------------|
| `steering/workflow.md` | Always - core workflow logic |
| `steering/template.md` | When creating new Living Spec |
| `steering/maintenance.md` | When updating specs |
| `steering/drift-detection.md` | On `/spec drift` or after code changes |
| `steering/traceability.md` | On traceability questions, QA review |
| `steering/decisions.md` | When choosing approaches |

### Role-Based Views (load on `/spec view <role>`)

| File | Triggers |
|------|----------|
| `steering/views/developer.md` | "as developer", "my tasks", "what to work on" |
| `steering/views/manager.md` | "as manager", "project status", "timeline" |
| `steering/views/qa.md` | "as QA", "test coverage", "quality" |
| `steering/views/architect.md` | "as architect", "design decisions", "architecture" |

## AI-DLC Phases

| Phase | Icon | Purpose | Sections |
|-------|------|---------|----------|
| Planning | 🔵 | WHAT and WHY | 1. Intent, 2. Requirements, 3. Architecture |
| Building | 🟢 | HOW | 4. Implementation, 5. Metrics |
| Operating | 🟡 | RUN and MEASURE | 6. Decision Log, 7. Next Actions |

**Key Rules:**
- Approval gates required before phase transitions
- ISO timestamps on all updates
- Never auto-transition phases
- Never delete history (mark superseded instead)

## Directory Structure

```
.specs/
├── 00-project.living.md        # Main orchestrator (sorts first)
├── maintenance.md              # Maintenance steering (always loaded)
├── feature-auth/               # Feature spec (Option B)
│   ├── requirements.md
│   ├── design.md
│   └── tasks.md
└── feature-export/             # Another feature spec
    └── ...
```

## Status Icons

| Icon | Meaning |
|------|---------|
| ⬚ | Not started / Unanswered |
| 🔄 | In progress |
| ✅ | Complete |
| 🔵 | Planning phase |
| 🟢 | Building phase |
| 🟡 | Operating phase |

## Quick Reference

### Creating Living Spec (Options A/B)
1. Ask user for approach (A/B/C)
2. Create `.specs/maintenance.md` from maintenance template
3. Create `.specs/00-project.living.md` from template
4. For brownfield: Run codebase analysis first
5. Begin Planning phase

### Session Continuity
When returning to existing project with Living Spec:
```
Welcome back to [Project Name]!

Current State:
- Phase: [🔵/🟢/🟡]
- Next Action: [Action]
- Drift Score: [X]%

Options:
A) Continue where you left off
B) Review a previous section
C) Check feature spec statuses
D) Run drift detection
```

### Phase Transitions
Always require explicit approval:
```
🔵 Planning Complete
- Intent: [summary]
- Requirements: [X] questions answered
- Architecture: [Y] decisions approved

Ready to proceed to 🟢 Building? (yes/no)
```

## Behavior Rules

1. **Never skip approach selection** on first activation
2. **Always load workflow.md** for core logic
3. **Block on questionnaire** - don't proceed to Architecture until Requirements questions answered
4. **Approval gates are mandatory** - no auto-transitions
5. **Update timestamps** on every spec modification
6. **Calculate drift** after code changes to mapped files
7. **Offer spec update** after completing any work
8. **Use TodoWrite** to track tasks from the Execution Plan
