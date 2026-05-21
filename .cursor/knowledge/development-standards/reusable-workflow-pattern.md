# Reusable Workflow Pattern

## Purpose

Reusable workflows should be preferred over duplicated workflow variants with small behavioral differences.

The goal is to reduce:

- Duplicate Logic
- Maintenance Overhead
- Operational Inconsistency
- Review Complexity

while improving:

- Reusability
- Readability
- Supportability

---

## Standard

If multiple workflows perform the same core operation with only minor differences, prefer:

- A Single Reusable Workflow
- Parameterized Behavior
- Configurable Inputs

instead of creating multiple nearly identical workflows.

Preferred:

```text
GetTickets.xaml
  in_str_Status
```

Avoid:

```text
- GetSuccessTickets.xaml
- GetFailedTickets.xaml
- GetCancelledTickets.xaml
```

when the underlying logic is mostly identical.

## Why This Matters

Duplicated workflows increase:

- Maintenance Cost
- Bug Propagation Risk
- Operational Drift
- Review Noise
- Support Complexity

A fix applied to one workflow may be forgotten in another duplicated variant.

Over time, duplicated flows often evolve inconsistently and become operationally harder to maintain.

## Operational Risks

Workflow duplication may cause:

- Different Retry Behaviors
- Inconsistent Logging
- Different Exception Handling
- Diverging Business Logic
- Partial Bug Fixes
- Support Investigation Difficulty

This becomes especially problematic in:

- Queue-Based Processes
- API Integrations
- Reporting Flows
- Validation Logic

## Review Guidance

Check:

- Do Multiple Workflows Perform Nearly The Same Logic?
- Can Differences Be Represented Through Arguments/Config?
- Is Workflow Duplication Creating Operational Drift?
- Are Exception/Logging Behaviors Consistent Across Variants?

Flag as Suggestion if:

- Reusability Improvement Is Possible

Flag as Warning if:

- Duplicated Flows Already Behave Inconsistently
- Maintenance/Support Complexity Is Increasing

Flag as Critical only if:

- Duplicated Logic Causes Operational Corruption
- Different Variants Produce Inconsistent Business Results
