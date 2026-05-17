Fix Readiness Review Prompt

Read:

.review/rpa-review-result.md

Analyze which findings are safe for a future fix agent.

Do not modify project files.

For each finding, classify:

Auto-fixable
Semi-auto-fixable
Manual-only

Use these rules:

Auto-fixable

Only classify as Auto-fixable if the fix is mechanical and low-risk.

Examples:

typo correction
duplicate activity DisplayName improvement
file/class naming alignment when references can be safely updated
adding missing comments
adding non-invasive log message text
adding .gitkeep
updating review artifact formatting
argument prefix correction when direction is clear and all invokes can be safely updated
output variable initialization with String.Empty when low-risk and local
Semi-auto-fixable

Use Semi-auto-fixable when the agent can propose or prepare a patch but human approval is required.

Examples:

workflow modularization
exception type changes
config key refactoring
package cleanup
CI/CD file changes
queue reference logic
credential handling pattern changes
moving variables to narrower scopes
reversing If conditions to avoid empty Then / main logic in Else
adding DataTable row count guards before Add Queue Item
adding try-catch inside Invoke Code
Manual-only

Use Manual-only when fix requires business, production, security, or runtime knowledge.

Examples:

BusinessRuleException vs SystemException classification
retry behavior
Orchestrator asset setup
credential asset migration
queue uniqueness rules
selector validation
database idempotency
PDD alignment
changing framework catch output behavior
modifying queue transaction semantics

Write the fix readiness review to:

.review/rpa-fix-readiness.md

Reply in chat with only:

Fix readiness file path
Auto-fixable count
Semi-auto-fixable count
Manual-only count
