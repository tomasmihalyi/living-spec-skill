# Living Spec Hooks Template

Use this template to set up automation hooks for Living Spec projects.

## Overview

Claude Code hooks enable automatic actions at specific workflow points. Configure hooks in `.claude/settings.local.json` or via the `/hooks` command.

This template provides hooks for:
- Automatic drift detection after file changes
- Spec update reminders after code modifications
- Pre-commit spec validation

## Setup Instructions

1. Add hooks to `.claude/settings.local.json` in your project
2. Or use the `/hooks add` command to add hooks interactively
3. Hooks execute shell commands that can output messages to the conversation

## Important Notes

Claude Code hooks support these event types:
- `PreToolCall` - Before a tool is executed
- `PostToolCall` - After a tool completes
- `Notification` - For async notifications
- `Stop` - Before session ends

Hooks can:
- Run shell commands
- Output text to the conversation
- Block tool execution (PreToolCall only)

## Hooks Configuration

Add to `.claude/settings.local.json` in your project root:

```json
{
  "hooks": {
    "PostToolCall": [
      {
        "matcher": {
          "tool": "Write",
          "pathPattern": "src/**/*.{ts,tsx,js,jsx}"
        },
        "hooks": [
          {
            "type": "command",
            "command": "if [ -f .specs/00-*.living.md ]; then echo 'DRIFT_CHECK: Source file modified. Consider running /spec drift'; fi"
          }
        ]
      },
      {
        "matcher": {
          "tool": "Edit",
          "pathPattern": "src/**/*.{ts,tsx,js,jsx}"
        },
        "hooks": [
          {
            "type": "command",
            "command": "if [ -f .specs/00-*.living.md ]; then echo 'DRIFT_CHECK: Source file modified. Consider running /spec drift'; fi"
          }
        ]
      }
    ]
  }
}
```

### Alternative: Using /hooks Command

```bash
# Add drift detection hook interactively
/hooks add PostToolCall

# When prompted:
# - Matcher: tool=Write, pathPattern=src/**/*.{ts,tsx}
# - Command: echo 'DRIFT_CHECK: Consider /spec drift'
```

## Hook Descriptions

### PostToolUse: Drift Detection

**Trigger:** After any Write or Edit operation
**Purpose:** Track changes to files in Component Map
**Action:** Returns drift indicator for potential spec update

```json
{
  "matcher": "Write|Edit",
  "hooks": [
    {
      "type": "prompt",
      "prompt": "Check if modified file is in Living Spec Component Map...",
      "timeout": 15000
    }
  ]
}
```

### Stop: Spec Update Reminder

**Trigger:** Before Claude Code stops/ends session
**Purpose:** Remind developer to sync spec if code changed
**Action:** Returns reminder if spec update needed

```json
{
  "matcher": "*",
  "hooks": [
    {
      "type": "prompt",
      "prompt": "Check if spec update needed before stopping...",
      "timeout": 15000
    }
  ]
}
```

### SessionStart: Context Loading

**Trigger:** When Claude Code session begins
**Purpose:** Detect existing Living Spec for session continuity
**Action:** Returns spec path if exists

```json
{
  "matcher": "*",
  "hooks": [
    {
      "type": "command",
      "command": "if ls .specs/00-*.living.md...",
      "timeout": 5000
    }
  ]
}
```

## Advanced Hooks

### Pre-Commit Spec Validation

Add to your project's git hooks:

```json
{
  "PreToolUse": [
    {
      "matcher": "Bash",
      "hooks": [
        {
          "type": "prompt",
          "prompt": "If this is a git commit command, verify: 1) Living Spec drift < 50%, 2) All modified files are in Component Map. Return: {\"allowCommit\": boolean, \"issues\": []}",
          "timeout": 20000
        }
      ]
    }
  ]
}
```

### Comprehension Gate Enforcement

Force comprehension verification on phase transitions:

```json
{
  "SubagentStop": [
    {
      "matcher": "*",
      "hooks": [
        {
          "type": "prompt",
          "prompt": "If this subagent was working on phase transition, verify comprehension gate responses are logged in §6 Decision Log. Return: {\"comprehensionVerified\": boolean, \"missingResponses\": []}",
          "timeout": 15000
        }
      ]
    }
  ]
}
```

### Test Requirement Enforcement

Require tests for new implementations:

```json
{
  "Stop": [
    {
      "matcher": "*",
      "hooks": [
        {
          "type": "prompt",
          "prompt": "If code was written (Write/Edit used), check if corresponding tests exist. Return: {\"testsRequired\": boolean, \"untested\": [\"file paths\"]}",
          "timeout": 20000
        }
      ]
    }
  ]
}
```

## Environment Variables

Available in command hooks:

| Variable | Description |
|----------|-------------|
| `$CLAUDE_PROJECT_DIR` | Project root directory |
| `$CLAUDE_PLUGIN_ROOT` | Plugin directory (for portable scripts) |

## Best Practices

1. **Keep hooks fast** - Use 15-30 second timeouts
2. **Use prompt hooks for complex logic** - LLM-based decisions
3. **Use command hooks for simple checks** - File existence, etc.
4. **Test hooks thoroughly** - Validate JSON output format
5. **Don't block on non-critical checks** - Use async notifications

## Troubleshooting

### Hooks not loading
- Restart Claude Code session after changes
- Check JSON syntax with a validator
- Verify file path: `.claude/hooks/hooks.json`

### Hook timing out
- Increase timeout value
- Simplify prompt/command
- Check for infinite loops

### Hook returning wrong format
- Verify JSON output structure
- Test command separately
- Check prompt instructions
