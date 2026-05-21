Fix Readiness Review Prompt

Read:

.review/rpa-review-result.md

Analyze which findings are safe for a future fix agent according to:
- repository rules
- framework contracts
- runtime semantics
- development standards
- knowledge-layer operational guidance

Do not modify project files.

Before classifying a finding as Auto-fixable, validate whether the change can alter:
- framework lifecycle behavior
- runtime semantics
- reporting visibility
- transaction integrity
- supportability
- reconciliation behavior

The fix agent should behave conservatively.

Prefer preserving:
- framework contracts
- operational visibility
- transaction traceability
- reporting consistency
- runtime behavior

over aggressive automated fixing.

For each finding, classify:

- Auto-fixable
- Semi-auto-fixable
- Manual-only

---

# Auto-fixable

Only classify as Auto-fixable if the fix is:
- mechanical
- local
- low-risk
- operationally safe
- runtime-safe
- framework-safe

and does NOT alter:
- transaction lifecycle behavior
- reporting semantics
- exception visibility
- queue semantics
- retry behavior

Examples:
- typo correction
- duplicate activity DisplayName improvement
- file/class naming alignment when references can be safely updated
- adding missing comments
- adding non-invasive log message text
- adding .gitkeep
- updating review artifact formatting
- argument prefix correction when direction is clear and all invokes can be safely updated
- output variable initialization with String.Empty when low-risk and local
- activity naming improvements
- safe workflow parameter naming alignment

---

# Semi-auto-fixable

Use Semi-auto-fixable when:
- the agent can prepare/propose a patch
- but human approval is still required
- or operational impact is possible

Use Semi-auto-fixable if fix safety depends on:
- framework contracts
- runtime semantics
- operational assumptions
- production behavior
- business interpretation

Examples:
- workflow modularization
- exception type changes
- config key refactoring
- package cleanup
- CI/CD file changes
- queue reference logic
- credential handling pattern changes
- moving variables to narrower scopes
- reversing If conditions to avoid empty Then / main logic in Else
- adding DataTable row count guards before Add Queue Item
- adding try-catch inside Invoke Code
- logging structure improvements
- output array restructuring
- modifying output initialization timing
- changing exception wrapping logic
- modifying reporting flows
- changing output/report generation behavior

Any fix affecting:
- dt_Output
- output arrays
- reporting flow
- transaction status
- exception-path visibility

should default to Semi-auto-fixable unless fully mechanical and operationally safe.

---

# Manual-only

Use Manual-only when the fix requires:
- business knowledge
- production knowledge
- runtime validation
- operational approval
- security validation
- framework design decisions

Examples:
- BusinessRuleException vs SystemException classification
- retry behavior
- Orchestrator asset setup
- credential asset migration
- queue uniqueness rules
- selector validation
- database idempotency
- PDD alignment
- changing framework catch output behavior
- modifying queue transaction semantics
- changing reconciliation logic
- changing reporting contracts
- altering operational visibility behavior
- modifying framework lifecycle expectations

---

# Confidence Guidance

If fix safety depends on:
- business semantics
- runtime behavior
- framework assumptions
- production conditions
- operational expectations

lower confidence and prefer:
- Semi-auto-fixable
or
- Manual-only

over aggressive automation.

---

# Review Guidance

Do not classify a finding as Auto-fixable only because:
- the code change appears small
- the syntax change is simple
- the pattern looks mechanically replaceable

Always evaluate:
- operational impact
- framework impact
- runtime impact
- reporting visibility impact
- supportability impact

---

Write the fix readiness review to:

.review/rpa-fix-readiness.md

For every finding include:
- Fix Classification
- Reasoning
- Operational Risk
- Framework Impact
- Runtime Impact
- Confidence
- Human Approval Requirement

Reply in chat with only:

Fix readiness file path
Auto-fixable count
Semi-auto-fixable count
Manual-only count