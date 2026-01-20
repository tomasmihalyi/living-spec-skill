# Spec-Code Drift Detection

Load this steering when:
- User runs `/spec drift`
- User asks "is spec up to date?", "check drift", "spec outdated"
- After completing code changes to mapped files
- At session start to report drift status

## What is Drift?

Drift occurs when code changes without corresponding spec updates. High drift means the spec no longer accurately reflects the implementation.

## When to Check

1. **Automatically** after ANY code changes to files listed in §4 Component Map
2. **On request** via `/spec drift` command
3. **At session start** as part of welcome back

## Drift Score Calculation

```
drift_score = (files_modified_since_last_spec_update / total_mapped_files) × 100
```

### How to Calculate

1. **Get Last Updated timestamp** from Living Spec header
2. **List files in Component Map** (§4)
3. **Check each file's last modified time** against spec's Last Updated
4. **Count files modified after** spec update
5. **Calculate percentage**

### Example

```
Living Spec Last Updated: 2025-01-15T10:00:00
Component Map files: 10

Files modified after spec update:
- src/auth/login.ts (modified 2025-01-15T14:00:00) ✓ Changed
- src/auth/logout.ts (modified 2025-01-14T09:00:00) ✗ Not changed
- src/api/users.ts (modified 2025-01-15T16:00:00) ✓ Changed
... (8 more not changed)

Drift Score: 2/10 = 20%
```

## Thresholds and Actions

| Score | Status | Icon | Action |
|-------|--------|------|--------|
| 0-20% | Healthy | ✅ | Continue working normally |
| 21-50% | Review Needed | ⚠️ | Suggest spec update, don't block |
| 51-75% | Sync Required | 🔴 | Prompt for sync before new work |
| 76%+ | Critical | 🚨 | Block until spec updated |

## Detection Rules

### After Code Changes

When user completes any code modification:

1. **Check if file is in Component Map**
   - Yes → Increment potential drift
   - No → Ask if file should be added to Component Map

2. **Check if task was marked complete in spec**
   - Yes → Drift acceptable (expected change)
   - No → Flag for review

3. **Check for new files created**
   - Not in Component Map → Suggest addition

### Automatic Prompts

**At 21-50% (Review Needed):**
```
📊 Drift Score: [X]%

[Y] files have changed since the spec was last updated:
- `src/path/file1.ts`
- `src/path/file2.ts`

Should I update the Living Spec to reflect these changes?
- Update §4 Component Map
- Update §4 Execution Plan status
- Add entry to §6 Decision Log

(yes/no/later)
```

**At 51%+ (Sync Required):**
```
🔴 Drift Score: [X]% - Sync Required

The Living Spec is significantly out of date. [Y] of [Z] mapped files have changed.

Before continuing with new work, we should sync the spec.

Changes detected:
- `src/path/file1.ts` - [brief change description]
- `src/path/file2.ts` - [brief change description]
- ... [more files]

New files not in Component Map:
- `src/path/newfile.ts`

Let me update the spec now. (yes/skip at your own risk)
```

## Manual Check Command

When user runs `/spec drift` or asks "check drift":

```
📊 Spec-Code Drift Analysis

Living Spec: .specs/00-[project].living.md
Last Updated: [timestamp]

Component Map Files: [total]
├── Unchanged: [count] ✅
├── Modified: [count] ⚠️
└── New (unmapped): [count] ❓

Drift Score: [X]% [status icon]

Modified Files:
| File | Last Modified | Change Type |
|------|---------------|-------------|
| `src/file.ts` | [timestamp] | Content |
| `src/other.ts` | [timestamp] | Content |

Unmapped New Files:
- `src/new/file.ts`
- `src/another/new.ts`

Recommendations:
1. [Specific recommendation based on findings]
2. [Another recommendation]

Update spec now? (yes/no)
```

## Sync Process

When user approves sync:

1. **Update Component Map**
   - Add new files with descriptions
   - Update existing file descriptions if needed
   - Remove deleted files (mark as removed, don't delete entry)

2. **Update Execution Plan**
   - Mark relevant stages/tasks as complete
   - Update status icons

3. **Update Decision Log**
   - Add entry: "Spec synced with code changes"
   - Include list of changes

4. **Update Header**
   - Set new Last Updated timestamp
   - Update drift score to 0%

5. **Update Current Status**
   - Refresh Next Action
   - Update Last Completed

## Edge Cases

### Files Deleted
```
⚠️ Files in Component Map no longer exist:
- `src/old/file.ts` (deleted)

Remove from Component Map? (yes/keep for history)
```

### Major Refactor Detected
```
🔄 Major structural changes detected.

[X] files moved or renamed.
This may indicate a significant refactor.

Should I:
A) Update Component Map with new paths
B) Review changes before updating
C) Skip - I'll update manually
```

### Spec Not Found
```
❌ No Living Spec found at .specs/00-*.living.md

To create one, run: /spec
```

## Integration with Workflow

### During Building Phase

After each stage completion:
1. Calculate drift
2. If drift > 0%, prompt for update
3. Include stage completion in update

### At Phase Transitions

Before allowing transition:
1. Calculate drift
2. Require drift < 20% to proceed
3. Or explicit user override

## Preventing Drift

Best practices to minimize drift:

1. **Update spec immediately** after completing each task
2. **Add files to Component Map** as you create them
3. **Run `/spec drift`** before starting new work sessions
4. **Use TodoWrite** alongside spec updates
