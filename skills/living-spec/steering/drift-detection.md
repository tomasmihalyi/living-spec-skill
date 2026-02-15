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

### How to Calculate (Using Claude Code Tools)

1. **Get Last Updated timestamp** from Living Spec header:
   ```
   Use Read tool on .specs/00-*.living.md
   Extract "Last Updated:" line value
   ```

2. **List files in Component Map** (§4):
   ```
   Parse Component Map table from Living Spec
   Extract file paths from "Location" column
   ```

3. **Check each file's last modified time** using Bash:
   ```bash
   # macOS - get modification time as Unix timestamp
   stat -f '%m' src/path/to/file.ts

   # For multiple files in Component Map:
   for f in src/api/index.ts src/components/App.tsx; do
     if [ -f "$f" ]; then
       echo "$(stat -f '%m' "$f") $f"
     fi
   done
   ```

4. **Compare timestamps and calculate percentage**:
   - Convert spec "Last Updated" to Unix timestamp
   - Count files with modification time > spec timestamp
   - drift_score = (changed_files / total_mapped_files) × 100

5. **Find new unmapped files** using Glob:
   ```
   Glob pattern: src/**/*.{ts,tsx,js,jsx}
   Compare results against Component Map entries
   ```

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
4. **Use TaskCreate/TaskUpdate** alongside spec updates

## Implementation Reference

### Full Drift Check Script (Bash)

```bash
# Get spec last updated (ISO format from header)
SPEC_FILE=".specs/00-*.living.md"
SPEC_DATE=$(grep "Last Updated" $SPEC_FILE | head -1 | cut -d: -f2- | xargs)

# Convert to Unix timestamp (macOS)
SPEC_TS=$(date -j -f "%Y-%m-%dT%H:%M:%S" "$SPEC_DATE" "+%s" 2>/dev/null || echo "0")

# Check component files (extract from Component Map)
CHANGED=0
TOTAL=0

# Example for common locations - adapt to actual Component Map
for f in src/**/*.ts src/**/*.tsx; do
  if [ -f "$f" ]; then
    TOTAL=$((TOTAL + 1))
    FILE_TS=$(stat -f '%m' "$f")
    if [ "$FILE_TS" -gt "$SPEC_TS" ]; then
      CHANGED=$((CHANGED + 1))
      echo "Modified: $f"
    fi
  fi
done

# Calculate drift
if [ $TOTAL -gt 0 ]; then
  DRIFT=$((CHANGED * 100 / TOTAL))
  echo "Drift Score: ${DRIFT}%"
fi
```

### Using Glob for New File Detection

```
Pattern: src/**/*.{ts,tsx,js,jsx}
Purpose: Find all source files
Action: Compare against Component Map entries to find unmapped files
```
