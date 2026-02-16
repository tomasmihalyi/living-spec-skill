# Manager View

Load when user says:
- "as a manager", "manager view", "project status"
- "timeline", "progress report", "executive summary"
- "what's the status", "are we on track"

## Focus Sections

As a manager, you primarily care about:

| Section | What You Need |
|---------|---------------|
| Current Status | High-level state |
| §1 Intent | Problem we're solving, success criteria |
| §4 Execution Plan | Progress through stages |
| §5 Metrics | Business outcomes |
| §7 Next Actions | Blockers and priorities |

## Quick Answers

| Question | Where to Look | How to Answer |
|----------|---------------|---------------|
| "Are we on track?" | §4 Execution Plan + Phase | Compare progress to expectations |
| "What's blocking us?" | §7 Blocked | List blockers with owners |
| "When will it be done?" | §4 Execution Plan | Show remaining stages (no estimates) |
| "What's the business impact?" | §5 Business Metrics | Show targets vs current |
| "What decisions need my input?" | §3 Key Decisions | Show pending approvals |
| "What did we accomplish?" | §7 Recently Completed | Show recent wins |

## Manager Dashboard

When showing manager view:

```
📊 Manager View - [Project Name]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

🎯 EXECUTIVE SUMMARY

Phase: [🔵 Planning / 🟢 Building / 🟡 Operating]
Health: [✅ On Track / ⚠️ At Risk / 🔴 Blocked]

Problem: [One-line from §1]
Success Criteria: [Primary metric and target]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

📈 PROGRESS

Stages: [Completed]/[Total]
[████████░░░░░░░░] [X]%

| Stage | Status | Notes |
|-------|--------|-------|
| Stage 1 | ✅ | Complete |
| Stage 2 | ✅ | Complete |
| Stage 3 | 🔄 | In progress |
| Stage 4 | ⬚ | Not started |
| Stage 5 | ⬚ | Not started |

Current Focus: [Stage 3 goal]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

🚧 BLOCKERS & RISKS

| Blocker | Impact | Owner | Since |
|---------|--------|-------|-------|
| [Blocker] | [Impact] | @owner | [Date] |

[Or "No current blockers ✅"]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

⏳ PENDING DECISIONS

Decisions awaiting approval:
1. [Decision]: [Brief description]
   Options: A) [Option A] B) [Option B]

[Or "No pending decisions ✅"]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

📉 KEY METRICS

| Metric | Target | Current | Status |
|--------|--------|---------|--------|
| [Primary] | [Target] | [Current] | [Status] |
| [Secondary] | [Target] | [Current] | [Status] |

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

What would you like to do?
A) Deep dive on blockers
B) Review pending decisions
C) See team progress details
D) Generate status report
```

## Health Assessment

Calculate project health:

| Condition | Status |
|-----------|--------|
| No blockers + On current stage | ✅ On Track |
| Minor blockers OR drift 21-50% | ⚠️ At Risk |
| Critical blockers OR drift >50% | 🔴 Blocked |
| Phase gate not met but trying to proceed | 🔴 Blocked |

## Status Report Generation

When manager asks for status report:

```
📋 STATUS REPORT
Project: [Name]
Date: [ISO Date]
Prepared for: [Manager/Stakeholders]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

SUMMARY
[2-3 sentences on overall status]

PHASE: [Phase] ([X]% through phase)
HEALTH: [Status]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

ACCOMPLISHMENTS THIS PERIOD
- [Accomplishment 1]
- [Accomplishment 2]
- [Accomplishment 3]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

UPCOMING WORK
- [Next stage/milestone]
- [Following work]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

BLOCKERS & RISKS
[List or "None"]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

DECISIONS NEEDED
[List or "None"]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

METRICS
| Metric | Target | Current | Trend |
|--------|--------|---------|-------|
| [Metric] | [Target] | [Current] | [↑/↓/→] |

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

## Decision Support

When manager needs to make a decision:

```
📌 DECISION REQUIRED: [Decision Title]

Context:
[Why this decision is needed]

Options:
┌─────────────────────────────────────────┐
│ Option A: [Name]                        │
│ Pros: [Benefits]                        │
│ Cons: [Drawbacks]                       │
│ Impact: [Business/technical impact]     │
└─────────────────────────────────────────┘

┌─────────────────────────────────────────┐
│ Option B: [Name]                        │
│ Pros: [Benefits]                        │
│ Cons: [Drawbacks]                       │
│ Impact: [Business/technical impact]     │
└─────────────────────────────────────────┘

Recommendation: [If team has one]
Rationale: [Why]

Your decision: (A/B/need more info)
```

## Scope Change Handling

When scope change is proposed:

```
⚠️ SCOPE CHANGE REQUEST

Current Scope:
- [In scope items]

Proposed Addition:
- [New item]

Impact Assessment:
- Additional stages: [X]
- Affected components: [List]
- Risk level: [Low/Medium/High]

Options:
A) Approve - Add to current scope
B) Defer - Add to Future Considerations
C) Reject - Out of scope for this project
D) Trade-off - Add this, remove [something else]

Your decision:
```

## Red Flags for Managers

Alert on these conditions:

| Flag | Condition | Suggested Action |
|------|-----------|------------------|
| 🔴 Blocked | Blocker open >3 days | Escalate or reassign |
| ⚠️ Drift | Spec-code drift >50% | Request spec sync |
| ⚠️ Stale | No progress in >5 days | Check in with team |
| 🔴 Gate blocked | Phase gate requirements not met | Review blockers |
| ⚠️ Decisions pending | Decisions waiting >2 days | Prioritize decision |
