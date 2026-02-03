# Living Spec Hooks Template

Use this template to set up automation hooks for Living Spec projects.

## Overview

Claude Code hooks enable automatic actions at specific workflow points. This template provides hooks for:
- Automatic drift detection after file changes
- Spec update reminders before stopping
- Session start context loading
- Comprehension gate enforcement

## Setup Instructions

1. Create `.claude/hooks/` directory in your project
2. Copy the hooks.json content below
3. Customize matchers and prompts as needed
4. Restart Claude Code session to load hooks

## Hooks Configuration

Create `.claude/hooks/hooks.json`:

```json
{
  "description": "Living Spec automation hooks",
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Write|Edit",
        "hooks": [
          {
            "type": "prompt",
            "prompt": "A file was modified. Check if .specs/00-*.living.md exists and if the modified file is listed in §4 Component Map. Return JSON: {\"shouldCheckDrift\": boolean, \"file\": \"path\", \"inComponentMap\": boolean}",
            "timeout": 15000
          }
        ]
      }
    ],
    "Stop": [
      {
        "matcher": "*",
        "hooks": [
          {
            "type": "prompt",
            "prompt": "Before stopping, check: 1) Were Write/Edit tools used this session? 2) Does .specs/00-*.living.md exist? If both true, remind the user about updating the Living Spec. Return: {\"specUpdateNeeded\": boolean, \"reason\": \"summary of changes\"}",
            "timeout": 15000
          }
        ]
      }
    ],
    "SessionStart": [
      {
        "matcher": "*",
        "hooks": [
          {
            "type": "command",
            "command": "if ls .specs/00-*.living.md 1>/dev/null 2>&1; then echo '{\"hasSpec\": true, \"specPath\": \"'$(ls .specs/00-*.living.md)'\"}'; else echo '{\"hasSpec\": false}'; fi",
            "timeout": 5000
          }
        ]
      }
    ]
  }
}
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
