Full UiPath Project Review Prompt

# PHASE 1 — Knowledge Layer Initialization

Before reviewing any project files:

1. Read and internalize all files under:
   - .cursor/knowledge/uipath-runtime/
   - .cursor/knowledge/framework-behaviours/
   - .cursor/knowledge/development-standards/

2. Treat the knowledge layer as the operational source of truth.

3. Build the review perspective according to:
   - UiPath runtime semantics
   - framework lifecycle behavior
   - operational supportability expectations
   - transaction lifecycle integrity
   - reporting/reconciliation visibility
   - development standards
   - accepted framework behaviors

4. Before reviewing project files, generate an internal operational review summary containing:
   - Accepted Framework Behaviors
   - Runtime Caveats
   - Operational Priorities
   - Known False-Positive Patterns
   - Reporting/Reconciliation Assumptions
   - Framework-Owned Responsibilities

5. Use this synthesized operational perspective during the review.

6. Only after completing the knowledge synthesis phase, start analyzing project files.

The review must be:
- Knowledge-First
- Framework-Aware
- Runtime-Aware
- Operationally-Aware

Do NOT:
- Perform a generic SWE review first
- Generate findings first and adjust them later
- Apply generic .NET/C# assumptions without runtime/framework validation

If generic software engineering assumptions conflict with:
- UiPath runtime behavior
- framework contracts
- accepted framework behavior
- operational lifecycle semantics

prefer the knowledge-layer interpretation.

---

# PHASE 2 — Review Execution

Review this UiPath project according to:
- Repository Rules
- Framework Contracts
- Runtime Semantics
- Development Standards
- Operational Supportability Expectations
- Knowledge-Layer Guidance

Do not modify project files.

Write the full review result to:

.review/rpa-review-result.md

---

# Review Priorities

Prioritize findings in this order:

1. Runtime Semantics
2. Framework Contract Violations
3. Transaction Visibility Risks
4. Reporting/Reconciliation Risks
5. Operational Supportability
6. Queue Lifecycle Safety
7. Exception Lifecycle Consistency
8. Development Standards
9. Generic SWE Findings

Operational correctness is more important than stylistic purity.

---

# Primary Review Focus

Focus especially on:

- Production Readiness
- Runtime Semantics
- Framework Lifecycle Integrity
- Transaction Traceability
- Reporting Visibility
- Reconciliation Safety
- Supportability
- Queue Transaction Safety
- Logging Quality
- Exception Handling
- Output/Reporting Consistency
- Config Usage
- CI/CD Readiness
- UiPath-Specific Operational Patterns
- Dependency Hygiene
- Fix-Agent Readiness
- Hidden Failure Scenarios
- Operational Visibility Gaps
- Catch Path Consistency
- Output Array Safety
- Queue Reference Safety
- Runtime-Aware Dependency Interpretation
- Invoke Code Operational Safety

Also evaluate:
- If/Else Readability
- Variable Scope Optimization
- Suspicious Variables Used Only In Trivial Assignments
- Add Queue Item Row Count Validation
- Argument Direction Consistency
- Naming Consistency

only when they create meaningful operational or maintainability impact.

---

# Framework Ownership Rules

Do not generate findings against behaviors intentionally handled by the framework lifecycle.

If the framework explicitly guarantees:
- Reporting Behavior
- Output Handling
- Mail Behavior
- Transaction Finalization
- Queue Lifecycle Behavior
- Exception Lifecycle Routing
- Output Report Generation

avoid duplicate findings inside process workflows unless the process violates the framework contract.

Framework-owned behavior should not be treated as process-level design flaws unless direct operational risk exists.

---

# Accepted Behavior Suppression Rules

If a knowledge-layer document explicitly defines a behavior as:
- Accepted
- Framework-Owned
- Runtime-Specific
- Operationally Intentional

do not generate a finding against that behavior unless direct operational risk evidence exists.

Do not report findings only because:
- A generic SWE anti-pattern exists
- A generic .NET recommendation exists
- A generic best practice exists

Knowledge-layer interpretation overrides generic assumptions.

---

# Dependency Interpretation Rules

Before reporting dependency/package findings:

Apply:
.cursor/knowledge/uipath-runtime/package-dependency-resolution.md

Do NOT assume a dependency failure only because:
- A package appears in XAML
- But does not appear directly in project.json

UiPath may resolve:
- Transitive Dependencies
- Indirect Package References
- Runtime Package Restores
- Studio Feed Dependencies

Only escalate dependency findings when there is evidence of:
- Analyzer Failure
- Restore Failure
- CI/CD Failure
- Runtime Failure
- Reproducibility Risk
- Clean-Machine Instability

Otherwise classify as:
- Warning
or
- Manual Check

instead of Critical runtime failure.

---

# Finding Requirements

Do not generate findings only because:
- A generic best practice exists
- A generic SWE anti-pattern exists
- A workflow is large
- Duplicated logic exists
- Catch ordering differs from standard .NET expectations

A finding should have at least one of:
- Operational Consequence
- Runtime Risk
- Framework Contract Violation
- Transaction Visibility Risk
- Reconciliation/Reporting Risk
- Supportability Impact
- Maintainability Impact
- Production-Readiness Impact

---

# Severity Rules

Do not classify an issue as Critical unless there is direct evidence of:
- Runtime Instability
- Transaction Corruption
- Reporting Corruption
- Operational Invisibility
- Reconciliation Failure
- Production Outage Risk
- Security Exposure
- Broken Framework Lifecycle Behavior

Avoid severity inflation.

Use evidence-based severity.

---

# Manual Checks

If validation requires:
- UiPath Studio Execution
- Orchestrator Behavior
- Runtime Execution
- Production Environment Access
- CI/CD Execution
- Package Restore Validation
- PDD/Business Confirmation
- Selector Validation
- External System Validation

place the finding under:

Manual Checks

Do not speculate beyond available evidence.

---

# Review Philosophy

The goal of this review is NOT:
- Generic Code Purity
- Textbook SWE Enforcement
- Style Policing

The goal IS:
- Operational Stability
- Transaction Integrity
- Framework Consistency
- Supportability
- Reporting Visibility
- Maintainability
- Production Safety

Prefer:
- Operationally Meaningful Findings

over:
- Low-Impact Stylistic Findings

---

# Required Finding Format

For every finding include:

- Severity
- Status
- Rule/Category
- File / Workflow
- Evidence
- Why It Matters
- Operational Impact
- Supportability Impact
- Framework Contract Impact
- Suggested Fix
- Confidence
- Fix Eligibility

---

# Knowledge Usage Reporting

At the beginning of the review output include:

Applied Knowledge Files:
- <knowledge file list>

Operational Review Summary:
- Accepted Framework Behaviors
- Runtime Caveats
- Operational Priorities
- Known False-Positive Patterns
- Framework-Owned Responsibilities

This section is mandatory.

---

After writing the file, reply in chat with only:

Review file path
Overall risk
Production readiness
Finding counts by severity