# Maintenance Steering Template

Create this file at `.specs/maintenance.md` when setting up a project with Living Specs.

---

```markdown
# Living Spec Maintenance

> This file guides AI assistants in maintaining the Living Spec. Always loaded with the spec.

## Source of Truth

The Living Spec at `.specs/00-[PROJECT_NAME].living.md` is the **single source of truth** for this project.

**Approach:** [A) Living Spec Only | B) Living Spec + Feature Specs]

## Session Start Protocol

When starting a new session in this project:

1. **Read the Living Spec** at `.specs/00-[PROJECT_NAME].living.md`
2. **Check Current Status** section for next action
3. **Calculate drift score** if code changes detected
4. **Present welcome back** message with options

## When to Update the Living Spec

| Trigger | Sections to Update |
|---------|-------------------|
| Task/stage complete | §4 Execution Plan, §7 Next Actions |
| New feature spec created | §2 Related Feature Specs |
| Architecture decision | §3 Key Decisions, §6 Decision Log |
| Scope change | §1 Intent (Scope), §6 Decision Log |
| Phase complete | Current Status header, §6 Decision Log |
| Technical debt found | §4 Tech Debt Register |
| Metric measured | §5 Metrics |
| Priority change | §7 Next Actions |
| Blocker identified | Current Status, §7 Blocked |
| Blocker resolved | Current Status, §7 Next Actions |

## Update Format Rules

### Timestamps
- Always use ISO format: `YYYY-MM-DDTHH:MM:SS`
- Update `Last Updated` in header on every modification

### Status Icons
| Icon | Meaning |
|------|---------|
| ⬚ | Not started / Unanswered / Unlinked |
| 🔄 | In progress |
| ✅ | Complete |

### Phase Icons
| Icon | Phase |
|------|-------|
| 🔵 | Planning |
| 🟢 | Building |
| 🟡 | Operating |

### History Preservation
- **Never delete** entries from Decision Log
- Mark superseded decisions as `[SUPERSEDED by Decision X]`
- Keep completed items in "Recently Completed" for context

## After Completing Work

Always ask:
```
Work completed. Should I update the Living Spec?

Updates needed:
- [ ] §4 Execution Plan: Mark [stage/task] complete
- [ ] §4 Component Map: Add/update [files]
- [ ] §6 Decision Log: Record [decision made]
- [ ] §7 Next Actions: Update priorities
- [ ] Current Status: Update Last Completed
```

## Drift Detection

After ANY code changes to files in Component Map:

1. Calculate drift score:
   ```
   drift_score = (files_changed_since_last_update / total_mapped_files) × 100
   ```

2. Take action based on score:
   | Score | Status | Action |
   |-------|--------|--------|
   | 0-20% | ✅ Healthy | Continue working |
   | 21-50% | ⚠️ Review | Suggest spec update |
   | 51%+ | 🔴 Sync Required | Prompt before continuing |

3. Update drift score in Current Status

## Phase Transitions

**Never auto-transition phases.** Always:

1. Verify all exit criteria met
2. Present summary to user
3. Ask for explicit approval
4. Log transition in Decision Log
5. Update phase in header

## Spec Hierarchy (Option B)

```
Living Spec (orchestrates at phase level)
└── 00-[PROJECT_NAME].living.md (🔵 Planning)
    ├── feature-auth/ (🟢 Building)
    ├── feature-export/ (🔵 Planning)
    └── feature-dashboard/ (🟡 Operating)
```

Update hierarchy when:
- New feature spec created
- Feature spec phase changes
- Feature spec completed/archived

## Current Project State

**Project:** [PROJECT_NAME]
**Current Phase:** 🔵 Planning
**Current Focus:** [Initial focus]
**Last Session:** [Date or "New project"]

## Quick Commands

| User Says | Action |
|-----------|--------|
| "spec status" | Show Current Status + drift score |
| "spec drift" | Calculate and report drift |
| "spec update" | Offer to sync spec with current state |
| "what's next" | Show §7 Current Focus |
| "view as [role]" | Load role-specific view |
```

---

## Customization Notes

Replace these placeholders when creating:
- `[PROJECT_NAME]` - Actual project name
- `[A) Living Spec Only | B) Living Spec + Feature Specs]` - Chosen approach
- `[Initial focus]` - First action after setup

Keep this file in sync with the Living Spec's Current Status section.
