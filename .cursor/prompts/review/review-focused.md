Focused UiPath Review Prompt

Review only the requested area of this UiPath project according to:
- knowledge-layer operational guidance
- repository rules
- framework contracts
- runtime semantics
- development standards

Do not modify project files.

Write the result to:

.review/rpa-review-result.md

Focus primarily on the user-requested review domain.

Possible focus areas:
- dispatcher flows
- performer flows
- queue handling
- logging quality
- exception handling
- config/security
- CI/CD readiness
- naming consistency
- dependency hygiene
- supportability
- If/Else structure
- variable scope
- output array handling
- add queue item validation
- invoke code safety
- argument direction consistency

Even in focused reviews:
- consider framework lifecycle behavior
- consider runtime semantics
- consider operational consequences
- consider supportability/reconciliation visibility

Do not apply generic .NET/C# assumptions directly without validating:
- UiPath runtime behavior
- framework semantics
- operational context

When knowledge-layer documents exist, prioritize those operational semantics over generic software engineering assumptions.

Operational impact is more important than stylistic purity.

Do not generate findings only because a generic best practice exists.

A finding should have at least one of:
- operational consequence
- maintainability impact
- runtime risk
- supportability impact
- framework contract violation
- reconciliation/reporting visibility risk

If the requested review scope is narrow:
- avoid reporting unrelated low-impact findings
- prioritize depth over breadth
- focus on operationally meaningful issues inside the requested scope

Use evidence-based severity.

Do not classify an issue as Critical unless there is direct evidence.

Place uncertain items under:
Manual Checks

For every finding include:
- Severity
- Evidence
- Operational Impact
- Supportability Impact
- Framework Contract Impact
- Suggested Fix
- Confidence

After writing the file, reply in chat with only:

Review file path
Overall risk
Main concern
Finding counts by severity