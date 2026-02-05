# Agent Index & Spawning Guide

Quick reference for all Living Spec agents and how to spawn them.

## Agent Directory

| Agent | Purpose | Phase | Model |
|-------|---------|-------|-------|
| `requirements-analyst` | Extract FR/NFR in EARS format | Planning | sonnet |
| `architecture-reviewer` | Analyze design patterns, constraints | Planning | sonnet |
| `risk-assessor` | Security, performance, tech debt | Planning | sonnet |
| `database-specialist` | Schema, queries, migrations | Building | haiku |
| `api-specialist` | Endpoints, contracts, errors | Building | haiku |
| `frontend-specialist` | Components, state, UX | Building | haiku |
| `security-specialist` | Auth, validation, threats | Building | sonnet |
| `test-specialist` | Test strategy, coverage | Building | haiku |
| `spec-critic` | Review alignment, find gaps | Quality | haiku |
| `comprehension-gate` | Verify developer understanding | Transitions | haiku |
| `spec-updater` | Maintain spec documents | Maintenance | haiku |

## Spawning Patterns

### Planning Phase (Parallel)

Spawn all three in ONE message for parallel execution:

```
Task 1: subagent_type=general-purpose, model=sonnet
        description="Requirements analysis"
        prompt="Read ~/.claude/skills/agents/requirements-analyst.md and analyze this project"

Task 2: subagent_type=general-purpose, model=sonnet
        description="Architecture review"
        prompt="Read ~/.claude/skills/agents/architecture-reviewer.md and analyze this project"

Task 3: subagent_type=general-purpose, model=sonnet
        description="Risk assessment"
        prompt="Read ~/.claude/skills/agents/risk-assessor.md and analyze this project"
```

### Building Phase (Conditional)

Spawn based on feature scope:

| Feature Has | Spawn Agent | Model |
|-------------|-------------|-------|
| Data model changes | database-specialist | haiku |
| API endpoints | api-specialist | haiku |
| UI components | frontend-specialist | haiku |
| Auth/sensitive data | security-specialist | sonnet |
| Always (after design) | test-specialist | haiku |

### Quality Gates

```
# After implementation
Task: spec-critic (model=haiku) - Review alignment

# Before phase transition
Task: comprehension-gate (model=haiku) - Generate verification questions
```

### Maintenance

```
# After code changes or high drift
Task: spec-updater (model=haiku) - Sync spec with code
```

## When to Use Which Model

| Model | Use For | Cost |
|-------|---------|------|
| **sonnet** | Complex analysis (requirements, architecture, security) | Higher |
| **haiku** | Simpler tasks (updates, formatting, basic review) | Lower |

## Common Spawning Examples

### Example 1: New Brownfield Project
```
# Single message with 3 parallel Tasks
1. requirements-analyst (sonnet) - Extract existing requirements
2. architecture-reviewer (sonnet) - Document current architecture
3. risk-assessor (sonnet) - Identify security/tech debt
```

### Example 2: New Feature Spec
```
# Analyze feature, then spawn relevant specialists
1. database-specialist (haiku) - If touching data model
2. api-specialist (haiku) - If adding/changing endpoints
3. test-specialist (haiku) - Always, for test strategy
```

### Example 3: Phase Transition
```
# Sequential (not parallel)
1. spec-critic (haiku) - Review completeness
2. comprehension-gate (haiku) - Generate questions
3. [User answers questions]
4. spec-updater (haiku) - Log responses, update status
```

## Prompt Templates

### Generic Agent Prompt
```
Read ~/.claude/skills/agents/[AGENT].md and apply its instructions to [CONTEXT].
Focus on [SPECIFIC ASPECT]. Output in the format specified in the agent file.
```

### With Context Passing
```
Read ~/.claude/skills/agents/[AGENT].md.

Context from previous analysis:
- Architecture: [summary]
- Key risks: [summary]

Apply instructions focusing on [SPECIFIC ASPECT].
```
