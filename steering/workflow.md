# Living Spec Workflow

## Activation Triggers

- User runs `/spec` command
- User mentions "living spec", "create spec", "project documentation"
- User starts new project and asks for planning help

## Multi-Agent Orchestration

Living Spec leverages specialized subagents for parallel analysis and domain expertise.

### Available Agents (in `~/.claude/skills/agents/`)

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

### Spawning Agents

Use the Task tool to spawn agents. Call multiple Task tools in a SINGLE message for parallel execution.

**Required parameters:**
- `subagent_type`: "general-purpose" (for all skill agents)
- `description`: Short description (3-5 words), e.g., "Requirements analysis"
- `prompt`: "Read ~/.claude/skills/agents/[agent-name].md and apply its instructions to [specific task]"

**Example - Parallel Planning Agents:**
Send one message with three Task tool calls:
1. Task: subagent_type=general-purpose, description="Requirements analysis", prompt="Read ~/.claude/skills/agents/requirements-analyst.md..."
2. Task: subagent_type=general-purpose, description="Architecture review", prompt="Read ~/.claude/skills/agents/architecture-reviewer.md..."
3. Task: subagent_type=general-purpose, description="Risk assessment", prompt="Read ~/.claude/skills/agents/risk-assessor.md..."

## First-Time Flow

### Step 1: Present Options (Use AskUserQuestion Tool)

Use the AskUserQuestion tool with this configuration:

```json
{
  "questions": [{
    "header": "Approach",
    "question": "Which spec approach fits your project?",
    "options": [
      {
        "label": "A) Living Spec Only (Recommended)",
        "description": "Best for MVPs, small teams (1-5), rapid iteration. Single spec + steering."
      },
      {
        "label": "B) Living Spec + Feature Specs",
        "description": "Best for multiple features, growing projects. Orchestrated feature specs."
      },
      {
        "label": "C) Feature Specs Only",
        "description": "Best for clear feature boundaries, simple projects. Individual specs only."
      }
    ],
    "multiSelect": false
  }]
}
```

**Do NOT proceed until user responds. This is a blocking requirement.**

### Step 2: Handle Selection

| Choice | Action |
|--------|--------|
| A | Create maintenance steering → Create Living Spec |
| B | Create maintenance steering → Create Living Spec → Set up feature spec orchestration |
| C | Exit this workflow, use simple feature spec per request |

### Step 3: Project Type Detection

Use AskUserQuestion tool:

```json
{
  "questions": [{
    "header": "Project Type",
    "question": "Is this a new project or existing codebase?",
    "options": [
      {"label": "Greenfield", "description": "Starting fresh - no existing code to document"},
      {"label": "Brownfield", "description": "Existing codebase to document and analyze"}
    ],
    "multiSelect": false
  }]
}
```

### Step 4: Setup Based on Type

**Greenfield:**
1. Create `.claude/spec-steering.md` using maintenance template
2. Update `CLAUDE.md` to reference spec-steering (for auto-loading)
3. Create `.specs/00-[project].living.md` using template
4. Set `Project Type: Greenfield`
5. Skip Project Context section
6. Begin Planning phase

**Brownfield:**
1. Run codebase analysis (see below)
2. Create `.claude/spec-steering.md` using maintenance template
3. Update `CLAUDE.md` to reference spec-steering (for auto-loading)
4. Create `.specs/00-[project].living.md` using template
5. Set `Project Type: Brownfield`
6. Auto-populate Project Context from analysis
7. Present findings for validation
8. Begin Planning phase after user confirms

### CLAUDE.md Integration

After creating spec-steering.md, add to project's CLAUDE.md:

```markdown
## Living Spec Integration

This project uses Living Specifications. At every session:
1. Read `.claude/spec-steering.md` for maintenance rules
2. Check `.specs/00-[PROJECT].living.md` for current state
3. Calculate drift score if code changes detected
4. Offer spec updates after completing work
```

## Brownfield Reverse Engineering

### Codebase Analysis Steps

1. **Scan Project Structure**
   ```
   - Identify root directories (src/, lib/, app/, etc.)
   - Detect package managers (package.json, requirements.txt, go.mod, Cargo.toml)
   - Find configuration files
   - Locate test directories
   ```

2. **Extract Architecture**
   ```
   - Folder structure patterns
   - Layer identification (routes, controllers, services, models)
   - Architecture style (MVC, microservices, monolith, serverless)
   ```

3. **Identify Tech Stack**
   ```
   - Languages (by file extensions and configs)
   - Frameworks (from dependencies)
   - Databases (from connection strings, ORM configs)
   - Cloud services (from SDK imports, config files)
   ```

4. **Find Entry Points**
   ```
   - Main files (main.ts, index.js, app.py)
   - API routes/handlers
   - Lambda/serverless functions
   - CLI entry points
   ```

5. **Detect Existing Specs**
   ```
   - Check for .specs/, docs/, specifications/
   - Look for README.md with requirements
   - Find ADR (Architecture Decision Records)
   ```

6. **Identify Technical Debt**
   ```
   - TODO/FIXME comments
   - Outdated dependencies
   - Missing test coverage
   - Code duplication patterns
   ```

### Present Analysis Results

```
Codebase Analysis Complete

Architecture: [detected pattern]
Tech Stack: [languages], [frameworks]
Database: [if detected]
Components: [X] discovered
Entry Points: [Y] found
Existing Docs: [Z] found
Potential Tech Debt: [N] items

I'll auto-populate the Living Spec with these findings.
Please review and correct any inaccuracies.

Proceed? (yes/no)
```

## Phase Execution

### 🔵 Planning Phase

**Purpose:** Define WHAT we're building and WHY

**Parallel Agent Orchestration:**

WHEN entering Planning phase, spawn these agents IN PARALLEL:

```
┌─────────────────────────────────────────────────────────────────┐
│                     PLANNING ORCHESTRATOR                        │
├─────────────────────────────────────────────────────────────────┤
│  ┌─────────────────┐  ┌─────────────────┐  ┌─────────────────┐ │
│  │  requirements-  │  │  architecture-  │  │  risk-          │ │
│  │  analyst        │  │  reviewer       │  │  assessor       │ │
│  │  (FR/NFR)       │  │  (patterns)     │  │  (threats)      │ │
│  └────────┬────────┘  └────────┬────────┘  └────────┬────────┘ │
│           │                    │                    │           │
│           └────────────────────┼────────────────────┘           │
│                                │                                │
│                    ┌───────────▼───────────┐                   │
│                    │  Synthesize into      │                   │
│                    │  Living Spec §1-§3    │                   │
│                    └───────────────────────┘                   │
└─────────────────────────────────────────────────────────────────┘
```

**Steps:**

1. **Parallel Analysis (spawn simultaneously):**
   - `requirements-analyst`: Extract functional and non-functional requirements
   - `architecture-reviewer`: Identify patterns, layers, constraints
   - `risk-assessor`: Security, performance, technical debt analysis

2. **Synthesize Results:**
   - Combine agent outputs into Intent section
   - Merge requirements into §2 (EARS format)
   - Consolidate architecture findings into §3

3. Fill Intent section:
   - Problem Statement (required)
   - Hypothesis (optional for MVPs)
   - Success Criteria (required)
   - Failure Triggers (recommended)
   - Scope boundaries (required)

4. **Generate Requirements (Project-Type Aware):**

   **FOR GREENFIELD:**
   - Generate 3-10 questions to determine WHAT to build
   - Questions define scope, tech choices, priorities
   - Each question must have:
     - Clear question text
     - Options (A/B/C or open-ended)
     - Answer field
     - Status (⬚/✅)

   **FOR BROWNFIELD:**
   - AUTO-POPULATE requirements from codebase analysis
   - Mark existing decisions as "✅ Inherited"
   - NO blocking questionnaire required
   - Generate a "Future Considerations" section (non-blocking, optional)
   - User answers future-direction questions only when creating feature specs

5. **Blocking Behavior (Greenfield Only):**
   ```
   ⚠️ STOP: Complete the Requirements Questionnaire before proceeding to Architecture.

   Questions remaining: [X]
   ```

   **For Brownfield, skip this gate** - architecture is already determined by existing code.
   Instead, present:
   ```
   ✅ Requirements auto-populated from codebase analysis.
   ✅ Architecture decisions inherited from existing code.

   Review the documented state and confirm accuracy.
   Ready to proceed? (yes/make corrections)
   ```

6. Document Architecture decisions:
   - Each decision needs:
     - Timestamp (ISO)
     - Context (why needed)
     - Options considered
     - Choice made
     - Rationale
     - Approval status

7. Get approval for architecture:
   ```
   Architecture decisions documented:
   1. [Decision 1] - [Choice]
   2. [Decision 2] - [Choice]

   Approve architecture to proceed to Building? (yes/request changes)
   ```

**Exit Criteria:**

**Greenfield:**
- [ ] Intent section complete
- [ ] All questionnaire questions answered (✅)
- [ ] Architecture decisions approved
- [ ] Risk assessment reviewed

**Brownfield:**
- [ ] Intent section complete (auto-populated, user-verified)
- [ ] Inherited decisions documented (✅ Inherited)
- [ ] Architecture documented from codebase
- [ ] Technical debt catalogued
- [ ] User confirmed accuracy of analysis

### 🟢 Building Phase

**Purpose:** Define HOW we're building it

**Domain Specialist Orchestration:**

WHEN entering Building phase for a feature, spawn relevant specialists IN PARALLEL:

```
┌─────────────────────────────────────────────────────────────────┐
│                     BUILDING ORCHESTRATOR                        │
├─────────────────────────────────────────────────────────────────┤
│  Analyze feature scope to determine which specialists needed:    │
│                                                                  │
│  ┌──────────┐ ┌──────────┐ ┌──────────┐ ┌──────────┐           │
│  │ database │ │ api      │ │ frontend │ │ security │           │
│  │ specialist│ │ specialist│ │ specialist│ │ specialist│          │
│  │ (schema) │ │ (endpoints)│ │ (UI)     │ │ (auth)   │           │
│  └────┬─────┘ └────┬─────┘ └────┬─────┘ └────┬─────┘           │
│       │            │            │            │                   │
│       └────────────┴─────┬──────┴────────────┘                   │
│                          │                                       │
│              ┌───────────▼───────────┐                          │
│              │  Synthesize into      │                          │
│              │  design.md            │                          │
│              └───────────┬───────────┘                          │
│                          │                                       │
│              ┌───────────▼───────────┐                          │
│              │  test-specialist      │                          │
│              │  (test strategy)      │                          │
│              └───────────────────────┘                          │
└─────────────────────────────────────────────────────────────────┘
```

**Which Specialists to Spawn:**

| If Feature Has... | Spawn |
|-------------------|-------|
| Data model changes | `database-specialist` |
| API endpoints | `api-specialist` |
| UI components | `frontend-specialist` |
| Auth/sensitive data | `security-specialist` |
| Always | `test-specialist` (after design) |

**Steps:**

1. **Parallel Domain Analysis:**
   - Spawn relevant specialists based on feature scope
   - Each specialist analyzes their domain
   - Wait for all to complete

2. **Synthesize Design:**
   - Combine specialist outputs into design.md
   - Resolve any cross-domain conflicts
   - Document integration points

3. Create Execution Plan:
   - Break into stages (3-10 based on complexity)
   - Each stage has:
     - Name
     - Goal
     - Status (⬚/🔄/✅)

4. Populate Component Map:
   - List all components/modules
   - Map to file paths
   - Brief descriptions

5. Set up Technical Debt Register:
   - Carry over from risk-assessor analysis
   - Add new items as discovered
   - Include severity and trigger conditions

6. Define Metrics:
   - Business metrics with targets
   - Technical metrics (latency, error rate, etc.)

7. **Spawn test-specialist:**
   - Generate test strategy
   - Link tests to requirements (traceability)
   - Define coverage targets

8. **Use TaskCreate/TaskUpdate** to track stage execution:
   - Create todo item per stage
   - Mark in_progress when starting
   - Update spec status as completing
   - Mark completed when done

9. **After completing work, spawn spec-updater:**
   ```
   Work completed. Spawning spec-updater to sync Living Spec:
   - Update §4 Execution Plan status
   - Update §4 Component Map
   - Add to §6 Decision Log
   ```

10. **Spawn spec-critic after implementation:**
    - Review implementation against spec
    - Identify gaps and missing coverage
    - Generate comprehension questions

**Exit Criteria:**
- [ ] All stages complete (✅)
- [ ] Tests passing
- [ ] Component Map current
- [ ] Metrics defined
- [ ] Spec-critic review passed

### 🟡 Operating Phase

**Purpose:** RUN and MEASURE the solution

**Steps:**
1. Track deployment status
2. Fill current metric values
3. Validate assumptions with evidence
4. Log operational decisions
5. Capture learnings

**Activities:**
- Monitor metrics against targets
- Document incidents and resolutions
- Update Next Actions based on learnings
- Plan iterations based on data

## Phase Transitions

**Never auto-transition. Always require explicit approval AND comprehension verification.**

### Comprehension Gate Protocol

BEFORE any phase transition:

```
┌─────────────────────────────────────────────────────────────────┐
│                   TRANSITION GATE PROTOCOL                       │
├─────────────────────────────────────────────────────────────────┤
│  1. Verify exit criteria met                                     │
│  2. Spawn spec-critic for completeness review                   │
│  3. Spawn comprehension-gate for verification questions         │
│  4. Present questions to developer                              │
│  5. BLOCK until responses logged in §6 Decision Log             │
│  6. Only then allow transition                                   │
└─────────────────────────────────────────────────────────────────┘
```

**Why Comprehension Gates?**
Research shows AI assistance can reduce skill mastery by 17% when developers rubber-stamp without understanding. Comprehension gates ensure developers maintain conceptual mastery.

### Planning → Building

**Greenfield Transition:**
```
🔵 Planning Phase Complete

Summary:
- Intent: [one-line problem statement]
- Requirements: [X] questions answered
- Architecture: [Y] decisions approved
- Scope: [in-scope summary]

Checklist:
✅ Intent section complete
✅ Questionnaire complete
✅ Architecture approved
✅ Risk assessment reviewed
```

**Brownfield Transition:**
```
🔵 Planning Phase Complete (Brownfield)

Summary:
- Codebase: [X] components documented
- Architecture: [pattern] - inherited from existing code
- Tech Debt: [Y] items catalogued
- Decisions: [Z] inherited, user-verified

Checklist:
✅ Intent section complete (auto-populated)
✅ Inherited decisions documented
✅ Architecture documented from codebase
✅ Technical debt catalogued
✅ User verified accuracy

─────────────────────────────────────────────────────

📋 COMPREHENSION VERIFICATION

Before proceeding to Building, please answer these questions:

1. **Requirement Understanding**
   [Question about why a key requirement exists]

   Your response: _______________

2. **Architecture Reasoning**
   [Question about why the chosen approach was selected]

   Your response: _______________

3. **Trade-off Analysis**
   [Question about what we gave up with our choices]

   Your response: _______________

Your responses will be logged in §6 Decision Log.

After receiving responses, use AskUserQuestion to confirm transition:
```json
{
  "questions": [{
    "header": "Transition",
    "question": "Ready to proceed to Building phase?",
    "options": [
      {"label": "Yes, proceed", "description": "All requirements understood, move to Building"},
      {"label": "No, review more", "description": "Need to revisit some sections first"}
    ],
    "multiSelect": false
  }]
}
```

### Building → Operating

```
🟢 Building Phase Complete

Summary:
- Stages: [X/Y] complete
- Components: [Z] mapped
- Tests: [status]
- Spec-Critic Score: [X]%

Checklist:
✅ All stages complete
✅ Tests passing
✅ Metrics defined
✅ Spec-critic review passed

─────────────────────────────────────────────────────

📋 COMPREHENSION VERIFICATION

Before deploying, please answer these questions:

1. **Failure Mode Analysis**
   [Question about what happens when X fails]

   Your response: _______________

2. **Edge Case Handling**
   [Question about boundary conditions]

   Your response: _______________

3. **Debugging Readiness**
   [Question about how to debug specific scenarios]

   Your response: _______________

Your responses will be logged in §6 Decision Log.

Ready to deploy and proceed to 🟡 Operating? (yes/no)
```

### Code Review Gate

For significant implementations, spawn comprehension-gate:

```
📋 IMPLEMENTATION VERIFICATION

Before merging, please answer:

1. [Question about how this handles specific edge case]
2. [Question about what error you'd see if Y failed]
3. [Question about how you'd test this locally]

Your responses confirm understanding before merge.
```

## Session Continuity

When user returns to a project with existing Living Spec:

1. **Detect existing spec:**
   ```
   Check for .specs/00-*.living.md
   ```

2. **Present welcome back:**
   ```
   Welcome back to [Project Name]!

   Current State:
   - Phase: [🔵/🟢/🟡] [Phase Name]
   - Last Updated: [timestamp]
   - Next Action: [from §7]
   - Drift Score: [X]% [status emoji]

   Options:
   A) Continue where you left off
   B) Review a previous section
   C) Check feature spec statuses (Option B only)
   D) Run drift detection
   E) Start fresh conversation

   Your choice:
   ```

3. **Load context:**
   - Read the Living Spec
   - Load maintenance.md steering
   - Prepare relevant section for next action

## Updating Rules

### When to Update Living Spec

| Trigger | Update These Sections |
|---------|----------------------|
| Task/stage complete | §4 Execution Plan, §7 Next Actions |
| New feature spec created | §2 Related Feature Specs |
| Architecture decision made | §3 Key Decisions, §6 Decision Log |
| Scope change | §1 Intent (Scope), §6 Decision Log |
| Phase complete | Current Status, §6 Decision Log |
| Technical debt found | §4 Tech Debt Register |
| Metric measured | §5 Metrics |
| Priority change | §7 Next Actions |

### Update Format Rules

- **Timestamps:** Always ISO format (YYYY-MM-DDTHH:MM:SS)
- **Last Updated:** Update header on every modification
- **Status icons:** ⬚ (not started), 🔄 (in progress), ✅ (complete)
- **Phase icons:** 🔵 Planning, 🟢 Building, 🟡 Operating
- **Never delete:** Mark items as superseded, don't remove history

## Adaptive Complexity

Adjust depth based on project size:

**Greenfield Projects:**

| Complexity | Questionnaire | Decisions | Stages | Metrics |
|------------|---------------|-----------|--------|---------|
| Simple (1-2 weeks) | 3-5 questions | 1-2 | 3-5 | 2-3 |
| Moderate (1-2 months) | 5-10 questions | 3-5 | 5-10 | 4-6 |
| Complex (3+ months) | 10+ questions | 5+ | Multi-phase | 6+ |

**Brownfield Projects:**

| Complexity | Inherited Decisions | Tech Debt Items | Components | Future Considerations |
|------------|--------------------:|----------------:|-----------:|----------------------:|
| Simple | 3-5 auto-documented | 1-5 | 5-10 | 1-2 optional |
| Moderate | 5-10 auto-documented | 5-10 | 10-30 | 2-4 optional |
| Complex | 10+ auto-documented | 10+ | 30+ | 4+ optional |

## Creating Feature Specs (Option B)

When creating a feature spec, use EARS format with three files:

### Step 1: Create Directory
```
.specs/feature-[name]/
├── requirements.md     # EARS requirements
├── design.md           # Architecture decisions
└── tasks.md            # Implementation tracking
```

### Step 2: Write Requirements (EARS Format)

Use these templates for requirements.md:

| Type | Template |
|------|----------|
| Ubiquitous | THE `<system>` SHALL `<response>` |
| Event-Driven | WHEN `<trigger>` THE `<system>` SHALL `<response>` |
| State-Driven | WHILE `<state>` THE `<system>` SHALL `<response>` |
| Unwanted | IF `<condition>` THEN THE `<system>` SHALL `<response>` |
| Optional | WHERE `<feature>` THE `<system>` SHALL `<response>` |

### Step 3: Document Design

In design.md:
- Architecture overview (ASCII diagram)
- Key decisions with rationale
- Data model / schema
- API design
- Integration points

### Step 4: Track Tasks

In tasks.md:
- Implementation tasks with dependencies
- Files to create/modify/delete
- Test cases linked to requirements
- Progress tracking

### Step 5: Link in Living Spec

Update "Related Feature Specs" section in the Living Spec:

```markdown
| Feature | Path | Phase | Owner | Description |
|---------|------|-------|-------|-------------|
| [Name] | `.specs/feature-[name]/` | 🔵 | @owner | [Description] |
```

## Anti-Patterns to Avoid

- ❌ Skipping approach selection on first use
- ❌ (Greenfield) Proceeding to Architecture before questionnaire complete
- ❌ (Brownfield) Blocking on questionnaire - should auto-populate instead
- ❌ (Brownfield) Asking future-planning questions during initial documentation
- ❌ Auto-transitioning phases without approval
- ❌ Skipping timestamps on decisions
- ❌ Deleting history instead of marking superseded
- ❌ Duplicating task tracking (use TaskCreate/TaskUpdate, reference in spec)
- ❌ Creating feature specs without Living Spec orchestration (Option B)
- ❌ Ignoring drift score warnings
- ❌ Using free-form requirements instead of EARS format
- ❌ Creating single-file feature specs instead of requirements/design/tasks split
