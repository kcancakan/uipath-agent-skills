# Output Contract Behavior

## Applicability

This pattern applies only to frameworks/projects that maintain transactional output structures such as:

- Output DataTables
- Output arrays
- Reconciliation files
- Transaction result exports
- Summary mails
- Status update files
- Operational reporting contracts

If the project does not use transactional reporting/output structures, this knowledge item may not be applicable.

## Framework Assumption

In many REFramework-style or customized RPA frameworks, transaction results are not only represented by queue item status.

They may also be represented through additional output structures that are consumed later by:

- End Process workflows
- Reporting flows
- Mail notification flows
- Source file update flows
- Reconciliation processes
- Support dashboards

## Operational Risk

If output structures are not initialized or populated consistently across success, Business Exception, and System Exception paths, the framework may still complete technically while operational reporting becomes incomplete.

Possible symptoms:

- Blank report rows
- Missing failed transaction rows
- Wrong success/failure counts
- Output mail missing failed items
- Source file not updated correctly
- Support team cannot trace failed transaction details
- Reconciliation mismatch between queue status and report output

## Common Risk Pattern

A transaction fails before output data is created.

Example:

```text
Validate input
↓
Throw BusinessRuleException
↓
Output array/DataTable row is never created
↓
End Process sends incomplete report
```

This is especially risky when End Process assumes that output structures are already complete.

## Review Guidance

When reviewing frameworks with output contracts, check:

- Are output structures initialized before risky operations?
- Are output rows created for both successful and failed transactions?
- Are Business Exception paths represented in output?
- Are System Exception paths represented in output?
- Can null output values create blank report rows?
- Does End Process validate output completeness?
- Does queue status align with report status?
- Is reporting logic dependent on variables that may remain null?

## Severity Guidance

Flag as Warning if output/reporting inconsistency may reduce supportability.

Flag as Critical if missing output data can cause:

- Incorrect business reporting
- Missed failed transactions
- Reconciliation failure
- Wrong source file update
- Silent operational visibility loss

## Preferred Interpretation

Do not assume technical process completion means operational reporting is complete.

For frameworks with output contracts, review transaction visibility across:

- Success
- Business Rule Exception
- System Exception
- Retry-exhausted cases
- Early validation failures
