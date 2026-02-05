---
name: spec
description: Living Specifications for Claude Code - consolidate development artifacts into AI-maintainable spec files with AI-DLC principles. Use for project planning, requirements, architecture, drift detection, and role-based views.
argument-hint: "[command] or [feature description]"
disable-model-invocation: false
user-invocable: true
allowed-tools: Read, Write, Edit, Grep, Glob, Bash, Task, TaskCreate, TaskUpdate, TaskList, AskUserQuestion
---

# /spec - Living Specifications for Claude Code

Consolidate development artifacts into single, AI-maintainable specification files with AI-DLC principles. Now with **multi-agent orchestration** for faster, higher-quality specs.

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

**MANDATORY: On first trigger in a project, use AskUserQuestion before doing anything else.**

Use the AskUserQuestion tool with this configuration:

```json
{
  "questions": [{
    "header": "Approach",
    "question": "Which spec approach fits your project?",
    "options": [
      {
        "label": "A) Living Spec Only (Recommended)",
        "description": "Best for MVPs, small teams (1-5), rapid iteration. Creates single Living Spec + maintenance steering."
      },
      {
        "label": "B) Living Spec + Feature Specs",
        "description": "Best for multiple features, growing projects. Living Spec orchestrates individual feature specs (EARS format)."
      },
      {
        "label": "C) Feature Specs Only",
        "description": "Best for clear feature boundaries, simple projects. Individual specs per feature (basic workflow)."
      }
    ],
    "multiSelect": false
  }]
}
```

**Do NOT proceed until user responds.**

## Multi-Agent Architecture

Living Spec uses specialized subagents for parallel analysis and domain expertise.

### Agent Directory
Located at `~/.claude/skills/agents/`:

| Agent | Purpose | When Spawned |
|-------|---------|--------------|
| `requirements-analyst` | Extract FR/NFR in EARS format | Planning phase |
| `architecture-reviewer` | Analyze design patterns | Planning phase |
| `risk-assessor` | Security, performance, debt | Planning phase |
| `database-specialist` | Schema, queries, migrations | Building phase |
| `api-specialist` | Endpoints, contracts, errors | Building phase |
| `frontend-specialist` | Components, state, UX | Building phase |
| `security-specialist` | Auth, validation, threats | Building phase |
| `test-specialist` | Test strategy, coverage | Building phase |
| `spec-critic` | Review alignment, find gaps | After implementation |
| `comprehension-gate` | Verify developer understanding | Phase transitions |
| `spec-updater` | Maintain spec documents | After code changes |

### Parallel Orchestration

**Planning Phase - Spawn in Parallel using Task tool:**

Use multiple Task tool calls in a SINGLE message for parallel execution:

| Task Call | subagent_type | description | prompt |
|-----------|---------------|-------------|--------|
| 1 | general-purpose | Requirements analysis | Read ~/.claude/skills/agents/requirements-analyst.md and apply to this project |
| 2 | general-purpose | Architecture review | Read ~/.claude/skills/agents/architecture-reviewer.md and apply to this project |
| 3 | general-purpose | Risk assessment | Read ~/.claude/skills/agents/risk-assessor.md and apply to this project |

Wait for all agents to complete, then synthesize findings into Living Spec §1-§3.

**Building Phase - Domain Specialists:**

Spawn relevant specialists based on feature scope (use Task tool in parallel):

| If Feature Has | Agent to Spawn | Prompt Template |
|----------------|----------------|-----------------|
| Data model | database-specialist | Read ~/.claude/skills/agents/database-specialist.md and analyze data requirements |
| API endpoints | api-specialist | Read ~/.claude/skills/agents/api-specialist.md and design API contract |
| UI components | frontend-specialist | Read ~/.claude/skills/agents/frontend-specialist.md and review UI design |
| Auth/sensitive | security-specialist | Read ~/.claude/skills/agents/security-specialist.md and assess security |
| Always | test-specialist | Read ~/.claude/skills/agents/test-specialist.md and create test strategy |

Synthesize specialist outputs into design.md.

### Comprehension Gates (Critical)

**Purpose:** Prevent skill atrophy by verifying developer understanding.

BEFORE any phase transition:
1. Spawn spec-critic agent (via Task tool) to review completeness
2. Spawn comprehension-gate agent to generate verification questions
3. Use **AskUserQuestion tool** to present questions to developer
4. Log responses in §6 Decision Log
5. Only then allow transition

Use AskUserQuestion with multiSelect: false for each comprehension question.

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
| `~/.claude/skills/steering/hooks-template.md` | When setting up automation hooks |

### Role-Based Views (load on `/spec view <role>`)

| File | Triggers |
|------|----------|
| `~/.claude/skills/steering/views/developer.md` | "as developer", "my tasks", "what to work on" |
| `~/.claude/skills/steering/views/manager.md` | "as manager", "project status", "timeline" |
| `~/.claude/skills/steering/views/qa.md` | "as QA", "test coverage", "quality" |
| `~/.claude/skills/steering/views/architect.md` | "as architect", "design decisions", "architecture" |

## Tiered Approval System

Not all spec changes require the same oversight level.

### Tier 1: Autonomous (No Gate)
- Formatting and timestamp updates
- Task status changes (⬚ → 🔄 → ✅)
- Drift score updates
- Recently Completed additions

### Tier 2: Async Notification (Update + Notify)
- Component Map additions
- Technical Debt items
- Next Actions priorities
- Decision Log entries

### Tier 3: Synchronous Approval (Blocking)
- New requirements (FR-xxx, NFR-xxx)
- Architecture changes (§3)
- Phase transitions
- Scope changes
- Security modifications

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
- Comprehension verification at all transitions
- ISO timestamps on all updates
- Never auto-transition phases
- Never delete history (mark superseded instead)

## Directory Structure

```
project/
├── .claude/
│   ├── spec-steering.md        # Auto-loaded steering (via CLAUDE.md)
│   └── hooks/
│       └── hooks.json          # Optional: automation hooks
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
2. Ask user for project type (Greenfield/Brownfield)
3. Create `.claude/spec-steering.md` from maintenance template
4. Update `CLAUDE.md` to reference spec-steering
5. Create `.specs/00-project.living.md` from template

**Greenfield flow:**
6. Generate requirements questionnaire (blocking)
7. Get architecture decisions approved
8. Begin Building phase after approval

**Brownfield flow:**
6. Spawn parallel analysis agents
7. Auto-populate spec from codebase analysis
8. Mark decisions as "✅ Inherited"
9. User verifies accuracy (non-blocking)
10. Ready for Building phase or feature spec creation

### Creating Feature Spec (Option B)
1. Create `.specs/feature-[name]/` directory
2. Spawn domain specialists for analysis
3. Create `requirements.md` using EARS template
4. Create `design.md` for architecture decisions
5. Create `tasks.md` for implementation tracking
6. Link in Living Spec's "Related Feature Specs" section

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

### Phase Transitions (With Comprehension Gate)

**Greenfield:**
```
🔵 Planning Complete

Summary:
- Intent: [summary]
- Requirements: [X] questions answered
- Architecture: [Y] decisions approved

Comprehension Verification Required:
[Questions from comprehension-gate agent]

Answer all questions to proceed to 🟢 Building.
```

**Brownfield:**
```
🔵 Planning Complete (Brownfield)

Summary:
- Codebase: [X] components documented
- Architecture: [pattern] - inherited
- Tech Debt: [Y] items catalogued
- Decisions: [Z] inherited, verified

✅ Ready for Building phase or feature spec creation.
```

## Behavior Rules

1. **Never skip approach selection** on first activation
2. **Always load workflow.md** for core logic
3. **Greenfield: Block on questionnaire** - don't proceed to Architecture until Requirements questions answered
4. **Brownfield: Auto-populate, don't block** - skip questionnaire, inherit decisions from codebase
5. **Approval gates are mandatory** - no auto-transitions
6. **Comprehension gates required** - verify understanding at transitions
7. **Update timestamps** on every spec modification
8. **Calculate drift** after code changes to mapped files
9. **Offer spec update** after completing any work
10. **Use parallel agents** for Planning phase analysis
11. **Use domain specialists** for Building phase implementation
12. **Use spec-critic** after implementation to verify alignment
13. **Use EARS format** for all feature spec requirements
14. **Create .claude/spec-steering.md** for auto-loading
15. **Brownfield: Future planning is optional** - only ask future-direction questions when creating feature specs
