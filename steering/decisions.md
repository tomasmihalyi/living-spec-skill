# Approach Selection Guide

Load this steering when helping users choose between approaches A, B, or C.

## Decision Matrix

| Factor | A: Living Spec Only | B: Living Spec + Feature Specs | C: Feature Specs Only |
|--------|--------------------|-----------------------------|----------------------|
| Team size | 1-5 | 5-20 | Any |
| Project duration | Weeks-months | Months-years | Any |
| Feature count | 1-3 concurrent | 4+ concurrent | Any |
| Complexity | Low-Medium | Medium-High | Low |
| Documentation need | Medium | High | Low |
| Overhead | ~15 min setup | ~30 min setup | ~10 min/feature |
| Best for | MVPs, startups | Growing products | Simple features |

## When to Recommend A: Living Spec Only

**Strong indicators:**
- Small team (1-5 people)
- Single product focus
- Rapid iteration / MVP phase
- Limited time for documentation
- Startup or early-stage project

**Example projects:**
- MVP for a new product
- Internal tool for small team
- Prototype or proof of concept
- Solo developer project
- Hackathon project

**Pitch:**
> "Living Spec Only gives you a single source of truth without overhead. Perfect for moving fast while maintaining clarity on what you're building and why."

## When to Recommend B: Living Spec + Feature Specs

**Strong indicators:**
- Growing team (5-20 people)
- Multiple features in parallel
- Need to delegate feature work
- Longer-term project (6+ months)
- Enterprise or B2B product

**Example projects:**
- SaaS platform with multiple modules
- E-commerce with distinct features
- Enterprise software with teams
- Product with complex integrations
- Multi-phase product roadmap

**Pitch:**
> "Living Spec + Feature Specs lets you orchestrate at the project level while teams work independently on features. The Living Spec tracks phase-level progress while feature specs handle the details."

## When to Recommend C: Feature Specs Only

**Strong indicators:**
- Very simple, isolated features
- User prefers minimal process
- One-off additions to existing codebase
- Clear, well-defined requirements already
- Just need task tracking, not documentation

**Example projects:**
- Bug fix with clear scope
- Simple feature addition
- Well-specified client request
- Maintenance work

**Pitch:**
> "For straightforward work with clear requirements, individual feature specs keep things simple. No orchestration overhead."

## Questions to Help Decide

Ask these to guide the recommendation:

1. **Team size?**
   - 1-5 → Lean toward A
   - 5+ → Lean toward B
   - Solo + simple → Consider C

2. **How many features will be in flight?**
   - 1-2 → A works well
   - 3+ → B provides better organization
   - Just 1 simple one → C might suffice

3. **Project timeline?**
   - Weeks → A or C
   - Months → A or B
   - Ongoing → B

4. **Documentation needs?**
   - "Just need to track work" → C
   - "Want to remember decisions" → A
   - "Need team alignment + history" → B

5. **Will requirements change?**
   - Stable → C is fine
   - Evolving → A or B to track changes

## Hybrid Scenarios

### Starting with A, Moving to B

Common pattern for growing projects:

1. Start with Living Spec Only (Option A)
2. As features grow, create feature specs for new work
3. Living Spec becomes the orchestrator

**Transition trigger:** When you have 3+ distinct feature areas that need parallel work.

### Starting with C, Adding A

When simple features become a product:

1. Build initial features with Feature Specs Only
2. Realize need for overall project view
3. Create Living Spec to consolidate and orchestrate

**Transition trigger:** When you lose track of the big picture or decisions.

## Anti-Patterns

### Over-engineering (A/B when C suffices)
**Symptom:** 30 min spec setup for 2-hour feature
**Solution:** Use C for simple, well-defined work

### Under-engineering (C when A/B needed)
**Symptom:** Lost context, repeated discussions, scope creep
**Solution:** Upgrade to A or B

### Wrong orchestration level (tracking tasks in Living Spec)
**Symptom:** Living Spec has 50+ tasks
**Solution:** Move task details to feature specs, Living Spec tracks phases only

## Decision Flow

```
Start
  │
  ▼
Is this a simple, one-off feature with clear requirements?
  │
  ├─ Yes → Option C (Feature Specs Only)
  │
  └─ No
      │
      ▼
    Team size > 5 OR multiple parallel features?
      │
      ├─ Yes → Option B (Living Spec + Feature Specs)
      │
      └─ No → Option A (Living Spec Only)
```

## Post-Selection Setup

### Option A Selected
1. Create `.specs/maintenance.md`
2. Create `.specs/00-[project].living.md`
3. Begin with Planning phase
4. All work tracked in single Living Spec

### Option B Selected
1. Create `.specs/maintenance.md`
2. Create `.specs/00-[project].living.md`
3. Begin with Planning phase in Living Spec
4. Create feature spec directories as needed: `.specs/feature-name/`
5. Living Spec tracks feature specs at phase level

### Option C Selected
1. Create `.specs/[feature-name].md` for each feature
2. Use simplified template (Requirements, Design, Tasks)
3. No orchestration layer
4. Exit Living Spec workflow, use basic spec-driven approach
