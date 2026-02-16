# EARS Format Reference

EARS (Easy Approach to Requirements Syntax) provides unambiguous, testable requirements.

## Syntax Templates

| Type | Template | Use When |
|------|----------|----------|
| **Ubiquitous** | THE `<system>` SHALL `<response>` | Always-on behavior |
| **Event-Driven** | WHEN `<trigger>` THE `<system>` SHALL `<response>` | Triggered by events |
| **State-Driven** | WHILE `<state>` THE `<system>` SHALL `<response>` | State-dependent behavior |
| **Unwanted** | IF `<condition>` THEN THE `<system>` SHALL `<response>` | Error handling |
| **Optional** | WHERE `<feature>` THE `<system>` SHALL `<response>` | Feature flags |
| **Complex** | WHILE `<state>` WHEN `<trigger>` THE `<system>` SHALL `<response>` | Combined conditions |

## Examples

### Ubiquitous (Always On)
```
THE system SHALL display a list of all tasks
THE system SHALL validate all API inputs using Zod schemas
THE system SHALL persist tasks in SQLite database
```

### Event-Driven (Triggered)
```
WHEN user submits new task THE system SHALL create task with status "pending"
WHEN user clicks delete THE system SHALL remove the task permanently
WHEN application loads THE system SHALL fetch and display all tasks
```

### State-Driven (Conditional)
```
WHILE tasks are loading THE system SHALL display "Loading..." indicator
WHILE form is submitting THE system SHALL disable all inputs
WHILE no tasks exist THE system SHALL display "No tasks" message
```

### Unwanted (Error Handling)
```
IF task not found THEN THE system SHALL return HTTP 404
IF input validation fails THEN THE system SHALL return HTTP 400
IF backend unavailable THEN THE system SHALL display error banner
```

### Optional (Feature Flags)
```
WHERE dark mode enabled THE system SHALL use dark color scheme
WHERE offline mode THE system SHALL queue mutations for later sync
```

## ID Conventions

| Prefix | Type | Example |
|--------|------|---------|
| FR-xxx | Functional Requirement | FR-001, FR-010 |
| NFR-xxx | Non-Functional Requirement | NFR-001, NFR-010 |

## Quality Checklist

- [ ] Each requirement is testable
- [ ] No ambiguity in wording
- [ ] Avoids implementation details
- [ ] Uses consistent terminology
- [ ] Has clear trigger/response
