Full UiPath Project Review Prompt

Review this UiPath project according to the repository rules.

Do not modify project files.

Write the full review result to:

.review/rpa-review-result.md

Focus on:

Production readiness
Supportability
Logging quality
Exception handling
Queue transaction safety
Config usage
Naming consistency
CI/CD readiness
UiPath-specific standards
Dependency hygiene
Fix-agent readiness
If/Else branch readability and empty Then branch anti-patterns
Variable scope optimization
Suspicious variables used only in trivial assignments
Process catch output array assignment
Output array variable initialization
Add Queue Item row count validation
Invoke Code try-catch quality
Argument name and direction consistency

Use evidence-based severity.

Do not classify an issue as Critical unless there is direct evidence.

If something requires Studio, Orchestrator, CI/CD, PDD, or runtime validation, place it under Manual Checks.

After writing the file, reply in chat with only:

Review file path
Overall risk
Production readiness
Finding counts by severity
