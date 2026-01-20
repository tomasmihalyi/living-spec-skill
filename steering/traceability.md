# Traceability Management

Load this steering when:
- User asks about traceability, RTM, requirements matrix
- User asks "what requirements lack tests?", "trace requirements"
- QA review is needed
- Audit or compliance check

## Purpose

Traceability ensures every requirement can be traced through design, implementation, and testing. This creates accountability and helps identify gaps.

## Traceability Chain

```
Requirement (§2) → Design Decision (§3) → Task/Stage (§4) → Test
     PR-001    →      Decision 1      →     Stage 2    →  T-001
```

## Linking Rules

| From | To | Requirement |
|------|----|-------------|
| Requirement (§2) | Design (§3) | MUST link to ≥1 design section or decision |
| Design (§3) | Task/Stage (§4) | SHOULD link to ≥1 stage or task |
| Task/Stage (§4) | Test | SHOULD link to ≥1 test (when tests exist) |
| Test | Requirement | SHOULD trace back to ≥1 requirement |

## Status Values

| Status | Meaning | Action |
|--------|---------|--------|
| ⬚ Unlinked | Missing required connections | Needs attention |
| 🔄 Partial | Some links present, others missing | Complete links |
| ✅ Complete | Fully traced end-to-end | Good |

## Traceability Matrix Format

The matrix in §2 should look like:

```markdown
| Req ID | Design Ref | Task/Stage | Test ID | Status |
|--------|------------|------------|---------|--------|
| PR-001 | §3.1, D-001 | Stage 2 | T-001, T-002 | ✅ Complete |
| PR-002 | §3.2 | Stage 3 | - | 🔄 Partial |
| PR-003 | - | - | - | ⬚ Unlinked |
```

## Automatic Orphan Detection

Flag items missing connections:

### Orphaned Requirements
Requirements with no design reference:
```
⚠️ Orphaned Requirements (no design link):
- PR-003: [requirement text]
- PR-005: [requirement text]

These requirements have no corresponding architecture decisions.
Should I help identify which decisions relate to them?
```

### Orphaned Tasks
Tasks with no requirement link:
```
⚠️ Orphaned Stages/Tasks (no requirement link):
- Stage 4: [task description]

This work isn't linked to any requirement.
Should I help identify the related requirement, or is this tech debt?
```

### Orphaned Tests
Tests with no requirement link:
```
⚠️ Orphaned Tests (no requirement link):
- T-005: test_edge_case.ts
- T-008: test_performance.ts

These tests don't trace to any requirement.
Should I help categorize them?
```

## Updating the Matrix

### When Adding Requirements

1. Add to §2 Project-Level Requirements
2. Matrix auto-adds row with ⬚ status
3. Prompt: "New requirement PR-XXX added. Which design decisions relate to it?"

### When Making Design Decisions

1. Add to §3 Key Decisions
2. Prompt: "Decision D-XXX made. Which requirements does this address?"
3. Update matrix with design reference

### When Creating Tasks/Stages

1. Add to §4 Execution Plan
2. Prompt: "Stage X created. Which requirements and decisions does this implement?"
3. Update matrix with task reference

### When Writing Tests

1. After test file created
2. Prompt: "Test T-XXX added. Which requirement does this verify?"
3. Update matrix with test reference

## Traceability Report

When user asks for traceability status:

```
📋 Traceability Report

Requirements: [total]
├── Fully Traced: [count] ✅
├── Partially Traced: [count] 🔄
└── Unlinked: [count] ⬚

Coverage Summary:
| Chain | Coverage |
|-------|----------|
| Req → Design | [X]% |
| Design → Task | [Y]% |
| Task → Test | [Z]% |
| End-to-End | [W]% |

Gaps Identified:

Requirements without design:
- PR-003, PR-007

Tasks without tests:
- Stage 4, Stage 6

Recommendations:
1. [Specific recommendation]
2. [Specific recommendation]
```

## QA Review Mode

When user says "QA review" or "as QA":

```
🔍 QA Traceability Review

Test Coverage by Requirement:

| Req ID | Requirement | Tests | Coverage |
|--------|-------------|-------|----------|
| PR-001 | [brief] | T-001, T-002 | ✅ Good |
| PR-002 | [brief] | T-003 | ⚠️ Minimal |
| PR-003 | [brief] | - | ❌ None |

Untested Paths:
- [Specific scenario not covered]
- [Another gap]

Test Quality Flags:
- [ ] All happy paths tested
- [ ] Error cases covered
- [ ] Edge cases addressed
- [ ] Performance tested (if NFR)

Recommendations for QA:
1. Add tests for PR-003
2. Expand coverage for PR-002
3. [Other recommendations]
```

## Compliance Use

For audit or compliance (SOC 2, etc.):

```
📜 Compliance Traceability Report

Audit Period: [start] to [end]

Requirements Implemented: [X]
- All traced: [Y]
- Gaps: [Z]

Evidence Chain:
| Control | Requirement | Implementation | Verification |
|---------|-------------|----------------|--------------|
| [Control ID] | PR-001 | Stage 2, Component A | T-001 |

Missing Evidence:
- [List any gaps]

Export formats available:
- Markdown (current)
- CSV (for spreadsheet import)
```

## Best Practices

1. **Link as you go** - Don't wait until the end to establish traceability
2. **Review weekly** - Check for orphaned items regularly
3. **Use consistent IDs** - PR-XXX, D-XXX, T-XXX format
4. **Document "why"** - When something isn't linked, note why
5. **Automate where possible** - Use test naming conventions that reference requirements
