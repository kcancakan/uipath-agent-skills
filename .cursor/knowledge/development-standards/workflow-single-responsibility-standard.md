# Workflow Single Responsibility Standard

## Purpose

Each workflow should focus on one clear operational responsibility.

The goal is to improve:

- Readability
- Testability
- Maintainability
- Supportability
- Reviewability

---

## Standard

A workflow should ideally perform one logical responsibility.

Examples:

- Read Input Data
- Validate Transaction
- Send API Request
- Update Output File
- Generate Report

Avoid combining many unrelated responsibilities into a single workflow.

---

## Why This Matters

Large multi-purpose workflows become difficult to:

- Review
- Test
- Debug
- Support
- Modify Safely

Operationally, support engineers should be able to quickly understand:

- What The Workflow Does
- Which Step Failed
- Which Responsibility Is Affected

---

## Anti-Pattern

```text
One Workflow:
- Reads Config
- Reads Input
- Calls APIs
- Validates Business Rules
- Generates Report
- Sends Mail
```

This increases:

- Coupling
- Review Complexity
- Operational Risk
- Regression Risk

## Preferred Pattern

Separate responsibilities into focused workflows:

- ReadInput.xaml
- ValidateTransaction.xaml
- SendRequest.xaml
- GenerateReport.xaml
- SendOutputMail.xaml

Orchestrator/framework workflows may coordinate these flows centrally.

## Important Exception

Framework-level orchestration workflows may naturally contain:

- Coordination Logic
- Transaction Lifecycle Management
- Flow Routing

Do not classify orchestration-heavy workflows as problematic only because of size.

Instead evaluate:

- Is The Workflow Understandable?
- Is Responsibility Separation Still Clear?
- Can Failures Be Isolated Easily?
- Is Operational Debugging Still Practical?

## Review Guidance

Check:

- Does The Workflow Have A Clear Responsibility?
- Are Unrelated Concerns Mixed Together?
- Can Logic Be Extracted Into Reusable Flows?
- Is The Workflow Difficult To Review/Test?
- Does Workflow Size Hide Operational Risk?

Flag as Suggestion if:

- Separation Could Improve Readability

Flag as Warning if:

- Workflow Complexity Reduces Maintainability/Supportability

Flag as Critical only if:

- Workflow Complexity Creates Hidden Operational Risk
- Failures Become Difficult To Isolate
- Transaction Boundaries Become Unclear
- Modifications Become Unsafe
