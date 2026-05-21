# Logging Standard

## Purpose

Logging should improve operational traceability without creating unnecessary noise or exposing sensitive/personal data.

The goal is to support:

- Debugging
- Support investigation
- Transaction traceability
- Framework observability

while respecting data privacy and security expectations.

---

## Workflow Start and End Logs

Workflow start/end logs should not be manually added inside the workflow body.

Instead, they should be configured from workflow parameters/settings.

Expected configuration:

```text
Start Log:
  Only Invocation

End Log:
  Only Successful Return
```

This creates standardized flow start/end logging without manually adding Log Message activities.

## Why Manual Start/End Logs Should Be Avoided

Manual start/end logs may cause:

- Duplicated logs
- Inconsistent log naming
- Noisy traces
- Missed standard invocation behavior
- Unnecessary activity clutter

Preferred approach:

```text
Workflow settings
↓
Start Log = Only Invocation
End Log = Only Successful Return
```

Avoid:

```text
Log Message - Flow Started
...
Log Message - Flow Completed
```

unless there is a specific non-standard operational reason.

## Business/Operational Logs

Logs inside the workflow do not need to be unique for every minor action.

They should be used when they provide meaningful operational value, such as:

- Important decision points
- Exception handling
- Retry decisions
- External system/API calls
- Queue transaction state
- File update results
- Reporting/output generation

Avoid logging every small assignment or technical step if it does not help support or debugging.

## Exception Logs

In catch blocks, log the original exception detail.

Expected log content:

`exception.Message`

The log activity display name should describe the failing flow or step.

Examples:

- Log Error - Create Request Body Failed
- Log Error - Update Ticket Failed
- Log Error - Input Excel Update Failed

After logging the original technical detail, throw a short and meaningful exception message to the framework layer.

Preferred pattern:

```text
Log:
  exception.Message

Throw:
  New SystemException("Değer hesaplaması sırasında hata alındı. Seller ID: " + str_SellerId)
```

Do not lose the original technical error detail.

## Sensitive Data / GDPR Standard

Logs must not expose sensitive personal data.

Do not log:

- TCKN
- Full names when unnecessary
- Phone numbers
- Email addresses when unnecessary
- Addresses
- IBAN / bank details
- Raw personal identifiers
- Credentials
- Tokens
- Passwords
- Full request/response bodies containing personal data

Bad example:

`TCKN 12345678901 için hata alındı`

Better:

`Kişisel veri içeren işlem için doğrulama hatası alındı. Transaction Reference: {reference}`

Prefer non-sensitive identifiers such as:

- Queue reference
- Transaction id
- Masked business key
- Internal request id
- Workflow name
- Step name

## Log Context

Where useful and safe, logs may include:

- workflow name
- step name
- transaction reference
- queue item reference
- non-sensitive business key
- exception type
- exception message

Do not include sensitive values simply because they are available.

## Review Guidance

When reviewing logs, check:

- Are workflow start/end logs configured through workflow settings?
- Are manual start/end logs avoided?
- Are exception messages logged before wrapping/throwing?
- Are log activity names meaningful?
- Are logs operationally useful rather than noisy?
- Are sensitive/KVKK-related values excluded?
- Are transaction references used instead of personal identifiers?
- Are full API request/response bodies avoided unless sanitized?

## Severity Guidance

Flag as Suggestion if:

- logs are slightly noisy
- log messages could be clearer

Flag as Warning if:

- Workflow start/end logs are manually added instead of configured
- Exception logs do not preserve original error details
- Logs lack meaningful operational context
- Logs include unnecessary business data

Flag as Critical if:

- Logs expose personal data such as TCKN
- Logs expose credentials/tokens/passwords
- Logs include raw sensitive API payloads
- Missing logs make production incident investigation impossible
