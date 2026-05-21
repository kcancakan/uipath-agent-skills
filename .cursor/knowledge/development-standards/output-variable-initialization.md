# Output Variable Initialization Standard

## Purpose

Processes using the custom framework are expected to produce transaction-level reporting data through framework-managed output structures.

To ensure failed transactions remain reportable, output-related variables must be initialized with safe defaults before risky operations.

## Standard

Variables that may later appear in:

- Output arrays
- Reporting DataTables
- Output mails
- Reconciliation structures

should be initialized immediately after queue item extraction.

Recommended pattern:

```text
str_SellerId = String.Empty
str_Amount = String.Empty
str_TotalAmount = String.Empty

before:

- Config reads
- Calculations
- Validations
- API calls
- Invoke Code
- Risky business operations
```

## Why This Standard Exists

Exceptions may occur before output variables are populated.

If catch blocks attempt to create output arrays using null variables:

- Reporting may fail
- Transactions may disappear from reports
- Support visibility may be lost

The goal is not only runtime success.

The goal is preserving operational visibility even during failure paths.

## Anti-Pattern

```text
Calculate TotalAmount
↓
BRE occurs before calculation
↓
Catch creates output array using null TotalAmount
↓
Reporting fails
```

## Preferred Pattern

```text
Initialize output variables safely
↓
Populate progressively
↓
Catch paths remain report-safe
```

## Review Guidance

Flag as Warning if:

- Output variables are initialized late
- Catch paths depend on nullable output values
- Output/report visibility can be lost during early failures

Flag as Critical if:

- Failed transactions may disappear completely from reporting
- Output generation itself can fail during exception handling
