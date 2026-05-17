Focused UiPath Review Prompt

Review only the requested area of this UiPath project according to the repository rules.

Do not modify project files.

Write the result to:

.review/rpa-review-result.md

When reviewing, focus only on the area requested by the user, such as:

dispatcher flows
performer flows
queue handling
logging quality
exception handling
config/security
CI/CD readiness
naming consistency
dependency hygiene
supportability
If/Else structure
variable scope
output array handling
add queue item validation
invoke code safety
argument direction consistency

Use evidence-based severity.

Place uncertain items under Manual Checks.

After writing the file, reply in chat with only:

Review file path
Overall risk
Main concern
Finding counts by severity
