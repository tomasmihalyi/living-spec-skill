---
name: spec
description: Living Specifications for Claude Code - consolidate development artifacts into AI-maintainable spec files with AI-DLC principles. Use for project planning, requirements, architecture, drift detection, and role-based views.
argument-hint: "[command] or [feature description]"
disable-model-invocation: false
user-invocable: true
allowed-tools: Read, Write, Edit, Grep, Glob, Bash, TodoWrite
---

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
   Creates: Living Spec orchestrates individual feature specs (EARS format)

C) FEATURE SPECS ONLY
   Best for: Clear feature boundaries, simple projects
   Creates: Individual specs per feature (basic workflow)

Your choice (A/B/C):
```

**Do NOT proceed until user responds with A, B, or C.**

## Steering Files

Load these contextually based on workflow phase (located at `~/.claude/skills/steering/`):

| File | When to Load |
|------|--------------|
| `~/.claude/skills/steering/workflow.md` | Always - core workflow logic |
| `~/.claude/skills/steering/template.md` | When creating new Living Spec |
| `~/.claude/skills/steering/maintenance.md` | Template for project steering file |
| `~/.claude/skills/steering/drift-detection.md` | On `/spec drift` or after code changes |
| `~/.claude/skills/steering/traceability.md` | On traceability questions, QA review |
| `~/.claude/skills/steering/decisions.md` | When choosing approaches |
| `~/.claude/skills/steering/ears-template.md` | When creating feature specs (Option B) |

### Role-Based Views (load on `/spec view <role>`)

| File | Triggers |
|------|----------|
| `~/.claude/skills/steering/views/developer.md` | "as developer", "my tasks", "what to work on" |
| `~/.claude/skills/steering/views/manager.md` | "as manager", "project status", "timeline" |
| `~/.claude/skills/steering/views/qa.md` | "as QA", "test coverage", "quality" |
| `~/.claude/skills/steering/views/architect.md` | "as architect", "design decisions", "architecture" |

## Auto-Loading via .claude/

**IMPORTANT:** When setting up a project, create `.claude/spec-steering.md` which will be auto-loaded on every session via CLAUDE.md integration.

This enables:
- Automatic drift detection on session start
- Spec update prompts after code changes
- Session continuity without explicit `/spec` invocation

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
project/
├── .claude/
│   └── spec-steering.md        # Auto-loaded steering (via CLAUDE.md)
├── .specs/
│   ├── 00-project.living.md    # Main orchestrator (sorts first)
│   ├── feature-auth/           # Feature spec (Option B) - EARS format
│   │   ├── requirements.md     # EARS requirements
│   │   ├── design.md           # Architecture decisions
│   │   └── tasks.md            # Implementation tasks
│   └── feature-export/         # Another feature spec
│       └── ...
└── CLAUDE.md                   # Add spec-steering integration
```

## EARS Format (Feature Specs)

Feature specs use EARS (Easy Approach to Requirements Syntax) format:

| Type | Template | When to Use |
|------|----------|-------------|
| Ubiquitous | THE `<system>` SHALL `<response>` | Always-on behavior |
| Event-Driven | WHEN `<trigger>` THE `<system>` SHALL `<response>` | Triggered by events |
| State-Driven | WHILE `<state>` THE `<system>` SHALL `<response>` | State-dependent |
| Unwanted | IF `<condition>` THEN THE `<system>` SHALL `<response>` | Error handling |
| Optional | WHERE `<feature>` THE `<system>` SHALL `<response>` | Feature flags |

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
2. Create `.claude/spec-steering.md` from maintenance template
3. Update `CLAUDE.md` to reference spec-steering
4. Create `.specs/00-project.living.md` from template
5. For brownfield: Run codebase analysis first
6. Begin Planning phase

### Creating Feature Spec (Option B)
1. Create `.specs/feature-[name]/` directory
2. Create `requirements.md` using EARS template
3. Create `design.md` for architecture decisions
4. Create `tasks.md` for implementation tracking
5. Link in Living Spec's "Related Feature Specs" section

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
9. **Use EARS format** for all feature spec requirements
10. **Create .claude/spec-steering.md** for auto-loading
