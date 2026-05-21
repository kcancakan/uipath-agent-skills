# Reference-Type Runtime Semantics

## Generic Assumption

Many reviewers assume that `in_` arguments are read-only and cannot affect outer workflow state.

This assumption is not always correct in UiPath/.NET runtime behavior.

## Actual UiPath Behavior

Some object types are passed by reference semantics even when used as `in_` arguments.

Examples:

- DataTable
- Dictionary
- List
- JObject/JArray
- Custom reference objects

This means:

- Inner workflow modifications
- Row changes
- Object mutations
- Collection updates

may still affect the outer workflow state even if the argument direction is only `in_`.

## Why This Matters

AI/static reviewers may incorrectly assume:

- No shared-state mutation exists
- Transaction state is isolated
- Workflow side effects are impossible

when actual runtime behavior still mutates shared objects.

Operationally this can create:

- Hidden side effects
- Retry inconsistencies
- Unexpected reporting changes
- Transaction data corruption
- Difficult debugging scenarios

## Review Guidance

When reviewing `in_` arguments, also evaluate:

- Argument data type
- Mutable object behavior
- Reference-type semantics
- Inner workflow mutation patterns

Do not assume:

```text
in_ = immutable
```

for reference-type objects.

## Example Scenario

Workflow A passes:

`in_dt_TransactionData`

to Workflow B.

Workflow B:

- Adds rows
- Removes rows
- Updates columns

Even though the direction is `in_`, the outer DataTable may still be modified because the DataTable object itself is shared by reference.

## Preferred Interpretation

Argument direction alone is not enough to understand mutation behavior.

Also evaluate:

- Object mutability
- Reference semantics
- Shared-state side effects
- Transaction lifecycle implications
