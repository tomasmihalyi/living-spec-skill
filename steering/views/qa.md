# QA View

Load when user says:
- "as QA", "QA view", "quality view"
- "test coverage", "what needs testing", "testing status"
- "quality gates", "acceptance criteria"

## Focus Sections

As QA, you primarily care about:

| Section | What You Need |
|---------|---------------|
| §2 Requirements | What needs to be verified |
| §2 Traceability Matrix | Test coverage mapping |
| §4 Stage Details | Acceptance criteria |
| §5 Metrics | Quality metrics |
| §1 Success Criteria | Definition of done |

## Quick Answers

| Question | Where to Look | How to Answer |
|----------|---------------|---------------|
| "What needs testing?" | §2 Traceability + §4 Stages | Show untested requirements |
| "Is requirement X tested?" | §2 Traceability Matrix | Check test links |
| "What are acceptance criteria?" | §4 Stage Details | Show criteria for stage |
| "What's our test coverage?" | §2 Traceability Matrix | Calculate coverage % |
| "Are we ready to ship?" | §5 Metrics + Traceability | Verify all criteria met |

## QA Dashboard

When showing QA view:

```
🔍 QA View - [Project Name]

Phase: [🔵/🟢/🟡] [Phase Name]
Quality Gate: [✅ Passing / ⚠️ At Risk / 🔴 Failing]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

📊 TEST COVERAGE

Requirements Coverage:
[████████████░░░░] [X]% ([Y]/[Z] requirements have tests)

| Status | Count |
|--------|-------|
| ✅ Fully tested | [X] |
| 🔄 Partially tested | [Y] |
| ⬚ No tests | [Z] |

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

🚨 TESTING GAPS

Requirements without tests:
| Req ID | Requirement | Priority |
|--------|-------------|----------|
| PR-003 | [Description] | HIGH |
| PR-007 | [Description] | MEDIUM |

Stages without acceptance verification:
- Stage 4: [Stage name]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

✅ ACCEPTANCE CRITERIA STATUS

Current Stage: [Stage Name]
| Criterion | Status | Evidence |
|-----------|--------|----------|
| [Criterion 1] | ✅ | [Test/PR link] |
| [Criterion 2] | ⬚ | - |
| [Criterion 3] | 🔄 | Partial |

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

📈 QUALITY METRICS

| Metric | Target | Current | Status |
|--------|--------|---------|--------|
| Test Coverage | [X]% | [Y]% | [Status] |
| Bug Count | <[X] | [Y] | [Status] |
| Critical Bugs | 0 | [Y] | [Status] |

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

What would you like to do?
A) See full traceability matrix
B) Review untested requirements
C) Check acceptance criteria for a stage
D) Generate test plan
```

## Traceability Analysis

When QA asks for coverage details:

```
📋 TRACEABILITY ANALYSIS

Full Matrix:
| Req ID | Requirement | Design | Tasks | Tests | Status |
|--------|-------------|--------|-------|-------|--------|
| PR-001 | [Brief] | §3.1 | S1,S2 | T-001,T-002 | ✅ |
| PR-002 | [Brief] | §3.2 | S3 | T-003 | ✅ |
| PR-003 | [Brief] | §3.1 | S4 | - | ⬚ |
| PR-004 | [Brief] | - | - | - | ⬚ |

Coverage by Type:
| Requirement Type | Total | Tested | Coverage |
|------------------|-------|--------|----------|
| Functional | [X] | [Y] | [Z]% |
| Non-Functional | [X] | [Y] | [Z]% |
| Security | [X] | [Y] | [Z]% |

Orphaned Tests (no requirement link):
- [Test file/name]
- [Test file/name]

Recommendation:
1. Prioritize tests for PR-003 (HIGH priority, no tests)
2. Link or remove orphaned tests
3. Add design reference for PR-004
```

## Test Plan Generation

When QA asks for test plan:

```
📝 TEST PLAN - [Project Name]

Generated: [Date]
Phase: [Phase]
Scope: [Stages being tested]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

1. TEST SCOPE

Requirements in scope:
- PR-001: [Brief]
- PR-002: [Brief]
- PR-003: [Brief]

Out of scope:
- [Items explicitly not being tested]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

2. TEST CASES BY REQUIREMENT

### PR-001: [Requirement]

| TC ID | Test Case | Type | Priority |
|-------|-----------|------|----------|
| TC-001 | [Happy path scenario] | Functional | HIGH |
| TC-002 | [Error case] | Negative | MEDIUM |
| TC-003 | [Edge case] | Boundary | LOW |

### PR-002: [Requirement]

| TC ID | Test Case | Type | Priority |
|-------|-----------|------|----------|
| TC-004 | [Scenario] | [Type] | [Priority] |

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

3. NON-FUNCTIONAL TESTING

| Area | Test Approach | Criteria |
|------|---------------|----------|
| Performance | [Approach] | [Target from §5] |
| Security | [Approach] | [Criteria] |
| Accessibility | [Approach] | [Standard] |

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

4. TEST ENVIRONMENT

Requirements:
- [Environment need]
- [Data requirements]
- [Access requirements]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

5. EXIT CRITERIA

- [ ] All HIGH priority test cases passed
- [ ] No critical bugs open
- [ ] Coverage ≥ [X]%
- [ ] All acceptance criteria verified
```

## Acceptance Verification

When verifying acceptance criteria:

```
✅ ACCEPTANCE VERIFICATION - Stage [X]: [Name]

| # | Criterion | Status | Evidence | Verified By |
|---|-----------|--------|----------|-------------|
| 1 | [Criterion text] | ✅ Pass | [Link/description] | @[name] |
| 2 | [Criterion text] | ⬚ Not verified | - | - |
| 3 | [Criterion text] | ❌ Fail | [Issue description] | @[name] |

Summary:
- Passing: [X]/[Y]
- Failing: [Z]
- Not verified: [W]

Stage ready to complete? [Yes ✅ / No - [reason]]
```

## Quality Gate Check

Before phase transition:

```
🚦 QUALITY GATE CHECK - [Phase] → [Next Phase]

Requirements Coverage:
[████████████████] 100% ✅

Acceptance Criteria:
[██████████████░░] 90% ⚠️
- Stage 4 criterion 2 not verified

Test Results:
- Total: [X] tests
- Passing: [Y] ✅
- Failing: [Z] ❌
- Skipped: [W] ⏭️

Open Bugs:
- Critical: 0 ✅
- High: 2 ⚠️
- Medium: 5
- Low: 8

Quality Metrics:
| Metric | Target | Current | Status |
|--------|--------|---------|--------|
| Coverage | 80% | 85% | ✅ |
| Bug density | <5 | 3 | ✅ |

Gate Status: ⚠️ AT RISK

Blockers:
1. Stage 4 acceptance criterion not verified
2. 2 HIGH bugs open

Proceed anyway? (yes/no/fix first)
```

## Red Flags for QA

| Flag | Condition | Action |
|------|-----------|--------|
| 🔴 No tests | Requirement has no tests | Create test cases |
| ⚠️ Low coverage | Coverage <70% | Identify gaps |
| 🔴 Failing tests | Tests failing | Investigate failures |
| ⚠️ Orphaned tests | Tests without requirements | Link or document |
| 🔴 Unverified AC | Acceptance criteria not checked | Verify before completion |
