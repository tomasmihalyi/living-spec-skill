---
name: Living Specifications
description: Activates when the user works with project specifications, mentions "living spec", creates documentation, or plans features using AI-DLC methodology. Provides multi-agent orchestration for AI-maintainable specifications.
version: 2.3.0
---

# Living Specifications Skill

This skill auto-activates when working with project specifications and AI-DLC workflows.

## When Active

This skill provides context for:
- Managing `.specs/` directories and living specification files
- EARS format requirements (FR-xxx, NFR-xxx)
- AI-DLC phase workflows (Planning → Building → Operating)
- Spec drift detection and maintenance
- Multi-agent orchestration for spec creation

## Core References

Load these steering files as needed:

| File | Purpose |
|------|---------|
| `${CLAUDE_PLUGIN_ROOT}/skills/living-spec/steering/workflow.md` | Core workflow logic and orchestration |
| `${CLAUDE_PLUGIN_ROOT}/skills/living-spec/steering/template.md` | Spec templates (minimal/standard/enterprise) |
| `${CLAUDE_PLUGIN_ROOT}/skills/living-spec/steering/ears-reference.md` | EARS format syntax reference |
| `${CLAUDE_PLUGIN_ROOT}/skills/living-spec/steering/ears-template.md` | Feature spec template |
| `${CLAUDE_PLUGIN_ROOT}/skills/living-spec/steering/drift-detection.md` | Drift calculation details |
| `${CLAUDE_PLUGIN_ROOT}/skills/living-spec/steering/maintenance.md` | Tiered approval system |
| `${CLAUDE_PLUGIN_ROOT}/skills/living-spec/steering/decisions.md` | Approach selection (A/B/C) |
| `${CLAUDE_PLUGIN_ROOT}/skills/living-spec/steering/traceability.md` | Requirements traceability matrix |
| `${CLAUDE_PLUGIN_ROOT}/skills/living-spec/steering/hooks-template.md` | Git hooks setup template |

## Role-Based Views

| View | File |
|------|------|
| Developer | `${CLAUDE_PLUGIN_ROOT}/skills/living-spec/steering/views/developer.md` |
| Manager | `${CLAUDE_PLUGIN_ROOT}/skills/living-spec/steering/views/manager.md` |
| QA | `${CLAUDE_PLUGIN_ROOT}/skills/living-spec/steering/views/qa.md` |
| Architect | `${CLAUDE_PLUGIN_ROOT}/skills/living-spec/steering/views/architect.md` |

## Available Agents

Spawn via the Task tool using `subagent_type="living-spec-skill:<agent-name>"`:

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
| `spec-critic` | Review alignment, find gaps | All phases |
| `comprehension-gate` | Verify understanding | Transitions |
| `spec-updater` | Maintain spec documents | After changes |

## Key Rules

1. Use EARS format for all requirements
2. Sub-agents return text only - orchestrator writes files
3. Spec context goes directly in project's CLAUDE.md
4. Phase transitions require comprehension gates
5. Use tiered approval (Autonomous / Notify / Blocking)
6. Never delete Decision Log entries - mark superseded

## For Full Usage

Use the `/spec` command for interactive spec management.
