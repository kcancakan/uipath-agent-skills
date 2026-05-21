# Invoke Code BusinessRuleException Wrapping

## Purpose

This knowledge file defines how `BusinessRuleException` should be handled when business validation logic is executed inside UiPath `Invoke Code` activities.

## Runtime Behaviour

UiPath `Invoke Code` does not always propagate exceptions with their original exception type to the outer UiPath `TryCatch` activity.

If `UiPath.Core.BusinessRuleException` is thrown inside an `Invoke Code` activity, the outer workflow may receive a wrapped exception instead of directly receiving `BusinessRuleException`.

### Common wrapping behaviour

```text
Invoke Code
  throws UiPath.Core.BusinessRuleException
      ↓
UiPath invocation layer wraps exception
      ↓
Outer TryCatch may receive TargetInvocationException / wrapped exception
      ↓
Original BusinessRuleException may exist under InnerException
```

Do not assume that:

```text
Throw BusinessRuleException inside Invoke Code
    =
BusinessRuleException catch catches it outside
```

This assumption is not runtime-safe.

## Review Guidance

When reviewing `Invoke Code` activities:

- Check whether the `Invoke Code` body throws `new UiPath.Core.BusinessRuleException(...)`
- Or throws another business validation exception that the outer workflow expects to handle as a `BusinessRuleException`

If yes, verify that one of the following safe patterns exists:

- Explicit result/flag pattern
- Safe `InnerException` unwrapping in the outer `TryCatch`
- Runtime-tested exception routing with clear evidence

If none exists, report it as a runtime semantic risk.

## Preferred Pattern: Result/Flag Based BRE

The safest pattern is:

1. `Invoke Code` performs validation.
2. `Invoke Code` returns validation result through output arguments.
3. UiPath workflow throws `BusinessRuleException` outside `Invoke Code`.

### Example Invoke Code output

```csharp
out_IsBusinessRuleError = true;
out_BusinessRuleMessage = "Business validation failed because required field is missing.";
return;
```

### Example UiPath workflow logic after Invoke Code

```vb
If bool_IsBusinessRuleError Then
    Throw New BusinessRuleException(str_BusinessRuleMessage)
End If
```

### Why this is preferred

- `BusinessRuleException` is thrown at UiPath workflow level
- Outer `BusinessRuleException` catch can handle it deterministically
- Queue status, output report, and framework exception lifecycle remain consistent
- Runtime exception wrapping ambiguity is avoided

## Alternative Pattern: Safe InnerException Unwrapping

If `BusinessRuleException` must be thrown inside `Invoke Code`, the outer `TryCatch` must safely inspect the wrapped exception.

### Unsafe pattern

```text
exception.InnerException.ToString.Contains("BusinessRuleException")
```

Do not rely on this pattern because:

- `InnerException` may be `Nothing`
- String matching is fragile
- Exception formatting may change
- A message may contain the same text and cause false positives
- Nested wrapping may require checking more than one inner exception level

### Safer pattern

```text
exception.InnerException IsNot Nothing AndAlso
TypeOf exception.InnerException Is UiPath.Core.BusinessRuleException
```

### Better pattern

```vb
currentException = exception

While currentException IsNot Nothing
    If TypeOf currentException Is UiPath.Core.BusinessRuleException Then
        Throw New BusinessRuleException(currentException.Message)
    End If

    currentException = currentException.InnerException
End While

Throw exception
```

The exact implementation may vary by workflow style, but the key requirement is:

- Null-safe check
- Type-based check
- Inner exception chain inspection if needed
- No string-based classification

## Risk

If `Invoke Code` throws `BusinessRuleException` but the outer workflow does not unwrap it correctly:

- Business validation failures may be treated as System Exceptions
- Queue items may be retried incorrectly
- Business failures may be reported as technical failures
- Output report rows may contain the wrong exception type
- DB/report/queue reconciliation may become inconsistent
- Operational support may investigate the wrong failure category

## Finding Guidance

Report this issue when:

- `Invoke Code` throws `BusinessRuleException` internally
- Outer workflow expects `BusinessRuleException` catch routing
- No explicit result/flag pattern exists
- No safe `InnerException` unwrap exists
- Existing classification relies on string matching

### Suggested severity

- **Warning** when the issue affects exception classification but no direct production evidence exists
- **Critical** only when there is direct evidence that queue status, DB status, reporting, or retry behaviour is already incorrect

## Fix Guidance

Recommended fix order:

1. Prefer replacing `Invoke Code` BRE throws with result/flag outputs
2. Throw `BusinessRuleException` from UiPath workflow level after `Invoke Code`
3. If this is not feasible, implement safe `InnerException` unwrapping
4. Add runtime validation proving BRE/SE routing behaves as expected

Do not auto-fix this blindly if it changes:

- Business/System exception classification
- Queue retry behaviour
- Reporting lifecycle
- DB reconciliation behaviour
- Framework exception routing

This is usually **Semi-auto-fixable** or **Manual-only** depending on business impact.

## Reviewer Reminder

`BusinessRuleException` thrown inside `Invoke Code` is not equivalent to `BusinessRuleException` thrown from a normal UiPath `Throw` activity.

Always validate how the exception is propagated before relying on outer `BusinessRuleException` catch behaviour.