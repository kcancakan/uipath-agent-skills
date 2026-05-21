# Then-Over-Else Pattern

## Purpose

In this development standard, the expected/default execution path should be implemented inside the `Then` branch whenever possible.

`Else` branches should preferably represent:

- Exceptional behavior
- Alternative paths
- Invalid conditions
- Fallback scenarios

instead of the primary business flow.

## Standard

Preferred structure:

```text
Condition = Valid State
↓
Then:
    Main business flow
Else:
    Exceptional/alternative behavior
```

Avoid:

```text
Condition = Invalid State
↓
Then:
    Fallback logic
Else:
    Main business flow
```

unless there is a strong readability reason.

## Why This Standard Exists

Large UiPath workflows are already visually complex because of:

- Nested Sequences
- Flowcharts
- TryCatch blocks
- Retries
- Queue handling
- Reporting logic

When the primary business flow is placed inside Else branches:

- Readability decreases
- Support/debugging becomes harder
- Cognitive load increases
- Operational review becomes slower
- Branch meaning becomes harder to follow

The goal is making the happy path visually easier to trace.

## Operational Impact

Support/debugging engineers usually follow:

- Expected execution flow
- Successful transaction flow
- Main business logic path

more frequently than exception paths.

Keeping the main flow inside Then branches improves:

- Supportability
- Debugging speed
- Operational readability
- Review consistency

## Example

### Anti-Pattern

```text
If InvalidSellerId
    Then:
        Throw BRE
    Else:
        Main process logic
```

### Preferred Pattern

```text
If ValidSellerId
    Then:
        Main process logic
    Else:
        Throw BRE
```

## Review Guidance

Flag as Suggestion if:

- Readability impact is small
- The flow remains understandable

Flag as Warning if:

- Large/nested workflows place most primary logic inside Else
- Supportability/readability is noticeably reduced

Do not classify as Critical unless:

- Branch structure creates operational misunderstanding
- Transaction logic becomes misleading
- Exception paths become indistinguishable from happy path behavior

## Exception Cases

This standard is a readability/maintainability convention.

Deviations may be acceptable when:

- The condition naturally expresses invalid state
- The flow becomes simpler with inverse logic
- Readability objectively improves
- Framework constraints require a different structure
