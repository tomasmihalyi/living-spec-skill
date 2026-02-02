# Living Spec Workflow

## Activation Triggers

- User runs `/spec` command
- User mentions "living spec", "create spec", "project documentation"
- User starts new project and asks for planning help

## First-Time Flow

### Step 1: Present Options (BLOCKING)

```
Which approach fits your project?

A) LIVING SPEC ONLY
   Best for: MVPs, small teams (1-5), rapid iteration
   Creates: Single Living Spec + maintenance steering
   Overhead: ~15 min setup, ~2 min per update

B) LIVING SPEC + FEATURE SPECS
   Best for: Multiple features, growing projects
   Creates: Living Spec orchestrates individual feature specs
   Overhead: ~30 min setup, scales with features

C) FEATURE SPECS ONLY
   Best for: Clear feature boundaries, simple projects
   Creates: Individual specs per feature
   Overhead: ~10 min per feature

Your choice (A/B/C):
```

**Do NOT proceed until user responds. This is a blocking requirement.**

### Step 2: Handle Selection

| Choice | Action |
|--------|--------|
| A | Create maintenance steering → Create Living Spec |
| B | Create maintenance steering → Create Living Spec → Set up feature spec orchestration |
| C | Exit this workflow, use simple feature spec per request |

### Step 3: Project Type Detection

Ask the user:
```
Is this a new project (greenfield) or existing codebase (brownfield)?

A) Greenfield - Starting fresh
B) Brownfield - Existing code to document
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

**Steps:**
1. Fill Intent section:
   - Problem Statement (required)
   - Hypothesis (optional for MVPs)
   - Success Criteria (required)
   - Failure Triggers (recommended)
   - Scope boundaries (required)

2. Generate Requirements Questionnaire:
   - Create 3-10 questions based on project complexity
   - Each question must have:
     - Clear question text
     - Options (A/B/C or open-ended)
     - Answer field
     - Status (⬚/✅)

3. **BLOCK until questionnaire complete:**
   ```
   ⚠️ STOP: Complete the Requirements Questionnaire before proceeding to Architecture.

   Questions remaining: [X]
   ```

4. Document Architecture decisions:
   - Each decision needs:
     - Timestamp (ISO)
     - Context (why needed)
     - Options considered
     - Choice made
     - Rationale
     - Approval status

5. Get approval for architecture:
   ```
   Architecture decisions documented:
   1. [Decision 1] - [Choice]
   2. [Decision 2] - [Choice]

   Approve architecture to proceed to Building? (yes/request changes)
   ```

**Exit Criteria:**
- [ ] Intent section complete
- [ ] All questionnaire questions answered (✅)
- [ ] Architecture decisions approved

### 🟢 Building Phase

**Purpose:** Define HOW we're building it

**Steps:**
1. Create Execution Plan:
   - Break into stages (3-10 based on complexity)
   - Each stage has:
     - Name
     - Goal
     - Status (⬚/🔄/✅)

2. Populate Component Map:
   - List all components/modules
   - Map to file paths
   - Brief descriptions

3. Set up Technical Debt Register:
   - Carry over from analysis (brownfield)
   - Add new items as discovered
   - Include severity and trigger conditions

4. Define Metrics:
   - Business metrics with targets
   - Technical metrics (latency, error rate, etc.)

5. **Use TodoWrite** to track stage execution:
   - Create todo item per stage
   - Mark in_progress when starting
   - Update spec status as completing
   - Mark completed when done

6. After completing work, always offer:
   ```
   Work completed. Should I update the Living Spec?
   - Update §4 Execution Plan status
   - Update §4 Component Map
   - Add to §6 Decision Log
   ```

**Exit Criteria:**
- [ ] All stages complete (✅)
- [ ] Tests passing
- [ ] Component Map current
- [ ] Metrics defined

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

**Never auto-transition. Always require explicit approval.**

### Planning → Building

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

Ready to proceed to 🟢 Building? (yes/no)
```

### Building → Operating

```
🟢 Building Phase Complete

Summary:
- Stages: [X/Y] complete
- Components: [Z] mapped
- Tests: [status]

Checklist:
✅ All stages complete
✅ Tests passing
✅ Metrics defined

Ready to deploy and proceed to 🟡 Operating? (yes/no)
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

| Complexity | Questionnaire | Decisions | Stages | Metrics |
|------------|---------------|-----------|--------|---------|
| Simple (1-2 weeks) | 3-5 questions | 1-2 | 3-5 | 2-3 |
| Moderate (1-2 months) | 5-10 questions | 3-5 | 5-10 | 4-6 |
| Complex (3+ months) | 10+ questions | 5+ | Multi-phase | 6+ |

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
- ❌ Proceeding to Architecture before questionnaire complete
- ❌ Auto-transitioning phases without approval
- ❌ Skipping timestamps on decisions
- ❌ Deleting history instead of marking superseded
- ❌ Duplicating task tracking (use TodoWrite, reference in spec)
- ❌ Creating feature specs without Living Spec orchestration (Option B)
- ❌ Ignoring drift score warnings
- ❌ Using free-form requirements instead of EARS format
- ❌ Creating single-file feature specs instead of requirements/design/tasks split
