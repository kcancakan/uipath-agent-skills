# Queue Reference Uniqueness Standard

## Purpose

Queue references should uniquely identify transactions while remaining short, readable, and operationally traceable.

---

## Standard

Queue references should:

- Be Unique Per Transaction
- Be Operationally Readable
- Stay Under 128 Characters
- Avoid Sensitive/KVKK Data

Preferred examples:

```text
- SellerId + "_" + InvoiceId
- OrderId
- RequestId
```

Avoid:

```text
- Static Values
- Generic Labels
- Long Concatenated References
- Personal/Sensitive Data
```

Bad example:

- AnalyseDocument
- Item + "_" + date.now

## Why This Matters

Queue references are used for:

- Retry Tracking
- Support Investigation
- Reporting
- Reconciliation
- Operational Debugging

Weak or duplicated references reduce transaction traceability and make support/debugging harder.

Overly long references may also create runtime/platform issues.

## Review Guidance

Check:

- Is The Reference Unique?
- Is It Readable?
- Is It Under 128 Characters?
- Does It Avoid Sensitive Data?
- Can Support Teams Identify The Transaction Easily?

Flag as Warning if:

- References Are Weak/Non-Unique
- References Are Excessively Long
- Operational Traceability Is Reduced

Flag as Critical only if:

- Queue Creation May Fail
- Transaction Ownership Becomes Ambiguous
- Retry/Reconciliation Reliability Is Lost
