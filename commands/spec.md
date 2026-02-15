---
name: spec
description: Living Specifications - consolidate development artifacts into AI-maintainable spec files with AI-DLC principles.
argument-hint: "[command] or [feature description]"
allowed-tools: Read, Write, Edit, Grep, Glob, Bash, Task, TaskCreate, TaskUpdate, TaskList, AskUserQuestion
---

# /spec - Living Specifications

Multi-agent orchestration for AI-maintainable specifications.

## Usage

```
/spec                    # Start new or continue existing
/spec <feature>          # Create spec for specific feature
/spec status             # View all specs and phases
/spec drift              # Check spec-code drift
/spec view <role>        # Role-based view (developer|manager|qa|architect)
```

## First-Time Activation

**MANDATORY:** Use AskUserQuestion on first trigger:

```json
{
  "questions": [{
    "header": "Approach",
    "question": "Which spec approach fits your project?",
    "options": [
      {"label": "A) Living Spec Only (Recommended)", "description": "MVPs, small teams, rapid iteration"},
      {"label": "B) Living Spec + Feature Specs", "description": "Multiple features, growing projects"},
      {"label": "C) Feature Specs Only", "description": "Clear feature boundaries, simple projects"}
    ],
    "multiSelect": false
  }]
}
```

Then ask Greenfield/Brownfield via AskUserQuestion.

## Core Files

**Always load:** `${CLAUDE_PLUGIN_ROOT}/skills/living-spec/steering/workflow.md` (core logic)

| File | Load When |
|------|-----------|
| `${CLAUDE_PLUGIN_ROOT}/skills/living-spec/steering/workflow.md` | Always |
| `${CLAUDE_PLUGIN_ROOT}/skills/living-spec/steering/template.md` | Creating new spec |
| `${CLAUDE_PLUGIN_ROOT}/skills/living-spec/steering/ears-reference.md` | Writing requirements |
| `${CLAUDE_PLUGIN_ROOT}/skills/living-spec/steering/drift-detection.md` | `/spec drift` |

## Multi-Agent Orchestration

**Planning Phase:** Spawn in parallel via Task tool:
- `living-spec-skill:requirements-analyst`
- `living-spec-skill:architecture-reviewer`
- `living-spec-skill:risk-assessor`

**Building Phase:** Spawn relevant specialists:
- `living-spec-skill:database-specialist`
- `living-spec-skill:api-specialist`
- `living-spec-skill:frontend-specialist`
- `living-spec-skill:security-specialist`
- `living-spec-skill:test-specialist`

**Quality Gates:**
- `living-spec-skill:spec-critic`
- `living-spec-skill:comprehension-gate`

**Maintenance:**
- `living-spec-skill:spec-updater`

### Spawning Pattern

Use the Task tool to spawn agents. Call multiple Task tools in a SINGLE message for parallel execution.

**Required parameters:**
- `subagent_type`: `"living-spec-skill:<agent-name>"` (plugin agent)
- `description`: Short description (3-5 words)
- `prompt`: Task-specific instructions

**Example - Parallel Planning Agents:**
Send one message with three Task tool calls:
1. Task: subagent_type=living-spec-skill:requirements-analyst, description="Requirements analysis", prompt="Analyze this project and extract requirements..."
2. Task: subagent_type=living-spec-skill:architecture-reviewer, description="Architecture review", prompt="Analyze this project's architecture..."
3. Task: subagent_type=living-spec-skill:risk-assessor, description="Risk assessment", prompt="Assess risks for this project..."

**Building Phase (Conditional):**

| Feature Has | Spawn Agent |
|-------------|-------------|
| Data model changes | `living-spec-skill:database-specialist` |
| API endpoints | `living-spec-skill:api-specialist` |
| UI components | `living-spec-skill:frontend-specialist` |
| Auth/sensitive data | `living-spec-skill:security-specialist` |
| Always (after design) | `living-spec-skill:test-specialist` |

## AI-DLC Phases

| Phase | Icon | Purpose |
|-------|------|---------|
| Planning | 🔵 | WHAT and WHY |
| Building | 🟢 | HOW |
| Operating | 🟡 | RUN and MEASURE |

## Tiered Approval

| Tier | Type | Examples |
|------|------|----------|
| 1 | Autonomous | Timestamps, status icons, drift |
| 2 | Notify | Component Map, tech debt, decisions |
| 3 | Blocking | Requirements, architecture, phase transitions |

## EARS Format

See `${CLAUDE_PLUGIN_ROOT}/skills/living-spec/steering/ears-reference.md` for syntax. Key templates:

- **Ubiquitous:** THE system SHALL [response]
- **Event:** WHEN [trigger] THE system SHALL [response]
- **Unwanted:** IF [condition] THEN THE system SHALL [response]

## Directory Structure

```
project/
├── CLAUDE.md                   # Project instructions + spec context (auto-loaded)
└── .specs/
    ├── 00-project.living.md    # Main spec
    └── feature-*/              # Feature specs (Option B)
        ├── requirements.md
        ├── design.md
        └── tasks.md
```

## Quick Reference

**Greenfield:** Questionnaire → Architecture approval → Building
**Brownfield:** Parallel analysis → Auto-populate → Verify → Building

**Phase Transitions:** Always require comprehension gate verification

## Behavior Rules

1. Never skip approach selection on first activation
2. Always load workflow.md for core logic
3. Greenfield: Block on questionnaire until complete
4. Brownfield: Auto-populate, non-blocking verification
5. Approval gates mandatory at phase transitions
6. Comprehension gates verify understanding
7. Use parallel agents for analysis
8. Use EARS format for requirements
9. Add spec context directly to CLAUDE.md (do NOT create separate steering files)
10. Sub-agents must return text output only - do NOT let them write files. The main orchestrator writes all spec files to `.specs/`

## CLAUDE.md Spec Section Template

When setting up specs, add this to the project's CLAUDE.md:

```markdown
## Specification
- **Main Spec:** `.specs/00-project.living.md`
- **Approach:** [A/B/C]
- **Phase:** [🔵 Planning | 🟢 Building | 🟡 Operating]

## Key Paths
[List important file paths]

## Development Guidelines
1. Before coding: Check spec for requirements
2. New features: Create feature spec in `.specs/feature-<name>/`
3. After changes: Update spec if architecture changes

## Tiered Approval
| Change Type | Approval |
|-------------|----------|
| Bug fixes, typos | Autonomous |
| New components, endpoints | Notify (update spec) |
| Architecture changes | Blocking (discuss first) |
```

## Status Icons

| Icon | Meaning |
|------|---------|
| ⬚ | Not started |
| 🔄 | In progress |
| ✅ | Complete |
