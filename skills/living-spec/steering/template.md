# Living Spec Template

Use this template to create `.specs/00-[project].living.md`

## Template Variants

Choose the appropriate template based on project complexity:

| Variant | Best For | Sections | Est. Setup |
|---------|----------|----------|------------|
| **Minimal** | MVPs, hackathons, POCs | Intent, Requirements (3-5 Q), Implementation (3-5 stages), Next Actions | Quick |
| **Standard** | Most projects, small teams | Full template below | Moderate |
| **Enterprise** | Compliance, audit trails | Standard + additional tracking sections | Comprehensive |

### Using Variants

Use AskUserQuestion to determine variant:
```json
{
  "questions": [{
    "header": "Template",
    "question": "Which template complexity fits your needs?",
    "options": [
      {"label": "Minimal (Recommended for MVPs)", "description": "Quick setup with essential sections only"},
      {"label": "Standard", "description": "Full Living Spec with all sections"},
      {"label": "Enterprise", "description": "Extended tracking for compliance/audit"}
    ],
    "multiSelect": false
  }]
}
```

---

## Minimal Template

For MVPs, hackathons, and proof-of-concepts. Creates a lightweight spec focused on essentials.

```markdown
# Living Spec: [Project Name]

> **Last Updated**: [YYYY-MM-DDTHH:MM:SS]
> **Phase**: 🔵 Planning | 🟢 Building | 🟡 Operating
> **Owner**: @[username]

## Current Status

| Next Action | Blockers | Drift Score |
|-------------|----------|-------------|
| [What to do next] | None | 0% ✅ |

---

## 1. Intent

### Problem
> [2-3 sentence problem description]

### Success Criteria
- [ ] [Primary success metric]
- [ ] [Secondary metric]

### Scope
**In:** [What's included]
**Out:** [What's excluded]

---

## 2. Requirements

### Inherited Decisions (Brownfield)
> Skip for greenfield. Auto-populated from codebase.

| Decision | Value | Status |
|----------|-------|--------|
| [Tech choice] | [detected] | ✅ Inherited |

### Key Questions (Greenfield Only)
> Skip for brownfield.

**Q1: [Most important question]**
Answer: _______________

**Q2: [Second question]**
Answer: _______________

**Q3: [Third question]**
Answer: _______________

---

## 3. Implementation

| Stage | Goal | Status |
|-------|------|--------|
| 1 | [First milestone] | ⬚ |
| 2 | [Second milestone] | ⬚ |
| 3 | [Third milestone] | ⬚ |

### Component Map

| Component | Location | Status |
|-----------|----------|--------|
| [Main component] | `src/path` | ⬚ |

---

## 4. Next Actions

- [ ] **[Current focus]**
- [ ] [Next item]

### Recently Completed
| Action | Date |
|--------|------|
| [Initial setup] | [Date] |

---

## Decision Log

| Date | Decision | Outcome |
|------|----------|---------|
| [Date] | Created spec | - |
```

---

## Standard Template

For most projects. Full Living Spec with all sections.

```markdown
# Living Spec: [Project/Feature Name]

> **Last Updated**: [YYYY-MM-DDTHH:MM:SS]
> **Phase**: 🔵 Planning | 🟢 Building | 🟡 Operating
> **Current Stage**: [Stage name within phase]
> **Project Type**: Greenfield | Brownfield
> **Approach**: A (Living Spec Only) | B (Living Spec + Feature Specs)
> **Owner**: @[username or team]

## Current Status

| Aspect | Value |
|--------|-------|
| **Next Action** | [What to do next] |
| **Blockers** | [Any blockers, or "None"] |
| **Last Completed** | [Last completed item] |
| **Drift Score** | [X]% [✅/⚠️/🔴] |

---

# 🔵 PLANNING PHASE

## 1. Intent

### Project Context (Brownfield Only)

> Skip this section for greenfield projects.

| Aspect | Current State | Source |
|--------|---------------|--------|
| Architecture | [Pattern: MVC/microservices/monolith/serverless] | [auto/manual] |
| Tech Stack | [Languages, frameworks, key libraries] | [auto/manual] |
| Key Dependencies | [Critical packages with versions] | [auto/manual] |
| Entry Points | [Main files, API routes, handlers] | [auto/manual] |
| Database | [Type and ORM if applicable] | [auto/manual] |
| Cloud/Infra | [AWS/GCP/Azure services, Docker, K8s] | [auto/manual] |

### Problem Statement

**Who:** [Who experiences this problem?]
**What:** [What is the problem?]
**Impact:** [What's the cost of not solving it?]

> [2-3 sentence problem description]

### Hypothesis (Optional)

| Element | Description |
|---------|-------------|
| Problem | [Core problem in one line] |
| Solution | [Proposed approach] |
| Customer | [Target user/persona] |
| Value Prop | [Why this solution wins] |
| Assumptions | [Key assumptions to validate] |

### Success Criteria

| Type | Criteria | Target | Current | Status |
|------|----------|--------|---------|--------|
| 🎯 Primary | [Main success metric] | [Target value] | - | ⬚ |
| 📈 Secondary | [Supporting metric] | [Target value] | - | ⬚ |
| 📈 Secondary | [Supporting metric] | [Target value] | - | ⬚ |

### Failure Triggers

> Conditions that would cause us to pivot or stop:

- [ ] [Condition 1 - e.g., "User adoption < 10% after 30 days"]
- [ ] [Condition 2 - e.g., "Cost exceeds $X/month"]
- [ ] [Condition 3 - e.g., "Performance < Y threshold"]

### Scope

**In Scope:**
- [What IS included]
- [Feature/capability 1]
- [Feature/capability 2]

**Out of Scope:**
- [What is NOT included]
- [Explicitly excluded item 1]
- [Explicitly excluded item 2]

**Future Considerations:**
- [Items for potential future phases]

---

## 2. Requirements

### Brownfield: Inherited Decisions

> **Auto-populated from codebase analysis. Review and confirm accuracy.**
> **Skip this section for greenfield projects.**

| Decision | Current Choice | Source | Status |
|----------|---------------|--------|--------|
| Database | [detected from codebase] | auto | ✅ Inherited |
| Framework | [detected from codebase] | auto | ✅ Inherited |
| Architecture | [detected from codebase] | auto | ✅ Inherited |
| Language | [detected from codebase] | auto | ✅ Inherited |

**Verification:** [ ] User has reviewed and confirmed inherited decisions are accurate

---

### Future Considerations (Brownfield - Optional)

> These questions are for planning FUTURE features. Non-blocking.
> Answer these when creating feature specs, not during initial documentation.

| Question | Current State | Future Direction | Priority |
|----------|--------------|------------------|----------|
| Authentication | [none/basic/oauth] | [keep/add/change] | [deferred] |
| Database | [current DB] | [keep/migrate] | [deferred] |
| Deployment | [current state] | [keep/cloud/container] | [deferred] |

---

### Requirements Questionnaire (Greenfield Only)

> **⚠️ STOP (Greenfield): Complete this questionnaire before proceeding to Architecture.**
> **ℹ️ (Brownfield): Skip this section - requirements auto-populated from codebase analysis.**

#### Q1: [Question Title]
**Question:** [Detailed question about a key requirement or decision]
**Options:**
- A) [Option A description]
- B) [Option B description]
- C) [Option C description]

**Your Answer:** `_______________`
**Rationale:** [Why this choice, filled after answering]
**Status:** ⬚ Unanswered | ✅ Answered

---

#### Q2: [Question Title]
**Question:** [Detailed question]
**Options:**
- A) [Option A]
- B) [Option B]

**Your Answer:** `_______________`
**Rationale:**
**Status:** ⬚ Unanswered

---

#### Q3: [Question Title]
**Question:** [Detailed question]
**Type:** Open-ended

**Your Answer:**
```
[Multi-line answer space]
```
**Status:** ⬚ Unanswered

---

### Questionnaire Status

| Total Questions | Answered | Remaining | Ready to Proceed? |
|-----------------|----------|-----------|-------------------|
| [X] | [0] | [X] | ❌ No |

---

### Project-Level Requirements

| ID | Requirement | Type | Priority | Status |
|----|-------------|------|----------|--------|
| PR-001 | [Requirement description] | Functional | HIGH | ⬚ |
| PR-002 | [Requirement description] | Functional | MEDIUM | ⬚ |
| PR-003 | [Requirement description] | Non-Functional | HIGH | ⬚ |

### Related Feature Specs (Option B Only)

> For projects using Living Spec + Feature Specs approach.

| Feature | Path | Phase | Owner | Description |
|---------|------|-------|-------|-------------|
| [Feature Name] | `.specs/feature-name/` | 🔵/🟢/🟡 | @owner | [Brief description] |

### Traceability Matrix

| Req ID | Design Ref | Task/Stage | Test ID | Status |
|--------|------------|------------|---------|--------|
| PR-001 | §3.1 | Stage 1 | T-001 | ⬚ Unlinked |
| PR-002 | - | - | - | ⬚ Unlinked |

> **Auto-maintained.** Items missing links are flagged with ⬚ Unlinked.

---

## 3. Architecture

### Approval Gate

> **⚠️ APPROVAL REQUIRED** before proceeding to Building phase.

| Status | Approved By | Date |
|--------|-------------|------|
| ⬚ Pending | - | - |

### System Overview

> High-level architecture description or diagram (ASCII/Mermaid).

```
[Component A] --> [Component B] --> [Component C]
      |                                   |
      v                                   v
  [Database]                         [External API]
```

[Narrative description of the architecture]

### Key Decisions

#### Decision 1: [Title]

| Aspect | Details |
|--------|---------|
| **Timestamp** | [YYYY-MM-DDTHH:MM:SS] |
| **Context** | [Why is this decision needed?] |
| **Options** | 1) [Option A] 2) [Option B] 3) [Option C] |
| **Choice** | [Selected option] |
| **Rationale** | [Why this choice?] |
| **Trade-offs** | [What we're giving up] |
| **Approval** | ⬚ Pending / ✅ Approved by @[name] |

---

#### Decision 2: [Title]

| Aspect | Details |
|--------|---------|
| **Timestamp** | [YYYY-MM-DDTHH:MM:SS] |
| **Context** | [Why needed?] |
| **Options** | 1) [A] 2) [B] |
| **Choice** | [Selected] |
| **Rationale** | [Why] |
| **Trade-offs** | [Cons] |
| **Approval** | ⬚ Pending |

---

### Technology Stack

| Layer | Technology | Version | Rationale |
|-------|------------|---------|-----------|
| Frontend | [Tech] | [Version] | [Why] |
| Backend | [Tech] | [Version] | [Why] |
| Database | [Tech] | [Version] | [Why] |
| Infrastructure | [Tech] | [Version] | [Why] |

---

# 🟢 BUILDING PHASE

## 4. Implementation

### Phase Gate: Planning → Building

> All items must be checked before entering Building phase.

**Greenfield Projects:**
- [ ] Intent section complete (Problem, Success Criteria, Scope)
- [ ] Questionnaire 100% answered
- [ ] Architecture decisions approved
- [ ] Traceability links established

**Brownfield Projects:**
- [ ] Intent section complete (auto-populated, user-verified)
- [ ] Inherited decisions reviewed and confirmed
- [ ] Architecture documented from codebase
- [ ] Technical debt catalogued

**Gate Status:** 🔒 Locked | 🔓 Unlocked
**Unlocked Date:** [YYYY-MM-DDTHH:MM:SS]

---

### Execution Plan

| Stage | Name | Goal | Dependencies | Status |
|-------|------|------|--------------|--------|
| 1 | [Stage Name] | [What this achieves] | - | ⬚ |
| 2 | [Stage Name] | [What this achieves] | Stage 1 | ⬚ |
| 3 | [Stage Name] | [What this achieves] | Stage 2 | ⬚ |
| 4 | [Stage Name] | [What this achieves] | Stage 1 | ⬚ |
| 5 | [Stage Name] | [What this achieves] | Stage 3, 4 | ⬚ |

### Stage Details

#### Stage 1: [Name]
**Goal:** [Detailed goal]
**Tasks:**
- [ ] [Task 1]
- [ ] [Task 2]

**Acceptance Criteria:**
- [ ] [Criterion 1]
- [ ] [Criterion 2]

**Status:** ⬚ Not Started | 🔄 In Progress | ✅ Complete
**Started:** -
**Completed:** -

---

### Component Map

| Component | Location | Description | Status |
|-----------|----------|-------------|--------|
| [Component] | `src/path/to/component` | [What it does] | ⬚ |
| [Component] | `src/path/to/component` | [What it does] | ⬚ |

### Technical Debt Register

| ID | Description | Severity | Trigger | Status |
|----|-------------|----------|---------|--------|
| TD-001 | [Debt description] | ⚠️ Medium | [When to address] | Open |
| TD-002 | [Debt description] | 🔴 High | [When to address] | Open |

### Dependency Tracking

| Dependency | Owner | Status | ETA | Notes |
|------------|-------|--------|-----|-------|
| [External dependency] | @team | ⬚ Waiting | [Date] | [Notes] |

---

## 5. Metrics

### Phase Gate: Building → Operating

> All items must be checked before entering Operating phase.

- [ ] All stages complete (✅)
- [ ] Tests passing
- [ ] No critical tech debt open
- [ ] Metrics collection configured

**Gate Status:** 🔒 Locked | 🔓 Unlocked

---

### Business Metrics

| Metric | Description | Target | Current | Status |
|--------|-------------|--------|---------|--------|
| [Metric Name] | [What it measures] | [Target] | - | ⬚ |
| [Metric Name] | [What it measures] | [Target] | - | ⬚ |

### Technical Metrics

| Metric | Description | Target | Current | Status |
|--------|-------------|--------|---------|--------|
| Latency (p50) | Median response time | [X]ms | - | ⬚ |
| Latency (p99) | 99th percentile | [Y]ms | - | ⬚ |
| Error Rate | % of failed requests | <[Z]% | - | ⬚ |
| Availability | Uptime percentage | [X]% | - | ⬚ |

### Monitoring Setup

| System | Purpose | Status |
|--------|---------|--------|
| [Tool] | [What it monitors] | ⬚ Not configured |

---

# 🟡 OPERATING PHASE

## 6. Decision Log

> Chronological log of all significant decisions and changes.

| Timestamp | Decision | Phase | Context | Outcome |
|-----------|----------|-------|---------|---------|
| [ISO] | Created Living Spec | 🔵 | Project kickoff | - |
| [ISO] | [Decision] | [Phase] | [Why] | [Result] |

---

## 7. Next Actions

### Current Focus (High Priority)

- [ ] **[Action 1]** - [Brief description]
- [ ] **[Action 2]** - [Brief description]

### Backlog (Medium Priority)

- [ ] [Action] - [Description]
- [ ] [Action] - [Description]

### Blocked

| Action | Blocker | Owner | Since |
|--------|---------|-------|-------|
| [Action] | [What's blocking] | @owner | [Date] |

### Recently Completed

| Action | Completed | Notes |
|--------|-----------|-------|
| [Action] | [Date] | [Any notes] |

---

## Appendix

### Glossary

| Term | Definition |
|------|------------|
| [Term] | [Definition] |

### References

- [Link to external doc]
- [Link to related resource]

### Change History

| Date | Author | Change |
|------|--------|--------|
| [Date] | @author | Initial creation |
```

---

## Template Usage Notes

1. **Delete sections marked (Brownfield Only)** for greenfield projects
2. **Delete sections marked (Greenfield Only)** for brownfield projects
3. **Delete sections marked (Option B Only)** if using Option A
4. **Greenfield:** Adjust questionnaire to project complexity (3-10 questions)
5. **Brownfield:** Auto-populate inherited decisions, skip questionnaire
6. **Adjust stages** to project size (3-10 stages)
7. **Add/remove metrics** based on what's measurable
8. **Replace all [placeholders]** with actual content
