# Retry Scope Runtime Behavior

## Generic Assumption

A Retry Scope is often interpreted as a safe wrapper that retries an action and eventually fails if the action cannot be completed.

However, this assumption may be incorrect when properties such as `ContinueOnError` are enabled.

## Actual UiPath Behavior

When `ContinueOnError=True` is used inside or around retry logic, failures may not propagate to the surrounding TryCatch structure.

This means the workflow can continue even if the retried operation failed after all retry attempts.

## Why This Matters

In RPA processes, retry failures are often operationally important.

If a failed operation is hidden by `ContinueOnError`, the process may:

- Complete without visible failure
- Mark a transaction as successful incorrectly
- Skip framework-level exception handling
- Fail to update source/output files
- Reduce support visibility
- Create reconciliation problems

This is especially risky for:

- Excel/file updates
- Queue status updates
- API calls
- Reporting outputs
- Irreversible business actions

## Review Guidance

When reviewing Retry Scope usage, check:

- Is `ContinueOnError=True` enabled?
- Is the retried action business-critical?
- Is there an explicit validation step after retry?
- Does failure propagate to the framework-level TryCatch?
- Is success/failure clearly logged?
- Can the process continue with stale or missing output?

Flag as Warning if retry failure may be hidden.

Flag as Critical if hidden retry failure can cause:

- Wrong transaction status
- Missing report/output update
- Duplicate business action
- Silent data inconsistency
- Support/reconciliation failure

## Example Scenario

A workflow writes transaction results back to an Excel file inside a Retry Scope.

If `ContinueOnError=True` is enabled and the write fails after retries, the process may continue as if the update succeeded.

Operationally, this can result in:

- Output mail sent
- Source file not updated
- Support team seeing inconsistent status

## Preferred Pattern

Retry failure should either:

- Propagate to the surrounding TryCatch, or
- Be followed by an explicit validation step that throws a meaningful exception if the operation did not succeed.

Example:

```text
Retry Scope
  Action: Write output file
  Condition: File exists / expected row count updated

If validation fails:
  Throw SystemException with meaningful context
```
