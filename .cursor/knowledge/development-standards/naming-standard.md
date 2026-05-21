# Naming Standard

## Purpose

Naming should improve:

- Readability
- Operational Traceability
- Supportability
- Reviewability
- Debugging Speed

Names should clearly describe:

- What The Object Represents
- What The Workflow Does
- What The Activity Performs

Avoid generic or misleading naming.

---

## Workflow Naming

### Standard

Workflow names should:

- Be Clear
- Be Descriptive
- Use PascalCase
- Avoid Spaces
- Represent Actual Responsibility

Preferred:

```text
- GetInvoiceData.xaml
- ValidateSellerId.xaml
- SendApiRequest.xaml
- UpdateOutputFile.xaml
```

Avoid:

```text
- Workflow1.xaml
- Test.xaml
- Get Data.xaml
```

### Why This Matters

Workflow names frequently appear in:

- Logs
- Exceptions
- Support Investigations
- Orchestrator Traces
- Debug Sessions
- Review Outputs

Weak names reduce operational visibility and debugging speed.

## Variable Naming

### Standard

Variables should follow:

`<TypePrefix>_<PascalCaseName>`

Examples:

- `str_SellerId`
- `int_RetryCount`
- `dt_Output`
- `arr_InvoiceIds`
- `dict_RequestHeaders`
- `bool_ShouldRetry`

### Why This Matters

Consistent naming improves:

- Readability
- Variable Type Visibility
- Review Speed
- Debugging
- Workflow Maintainability

This becomes especially important in:

- Large UiPath workflows
- Nested scopes
- Queue-based processes
- Support/debugging scenarios

## Argument Naming

### Standard

Arguments should follow:

`<Direction>_<TypePrefix>_<PascalCaseName>`

Examples:

- `in_str_SellerId`
- `out_dt_Output`
- `io_dict_RequestHeaders`

Argument direction should match actual usage.

Avoid:

- `io_` when mutation is unnecessary
- Misleading directions
- Generic names

### Why This Matters

Arguments define:

- Data Ownership
- Mutation Visibility
- Workflow Contracts
- Transaction Boundaries

Incorrect or misleading argument naming reduces operational clarity.

## Activity Naming

### Standard

Activities should have meaningful Display Names describing the actual operation.

Preferred:

- Assign - Initialize Output Variables
- Throw - Seller Id Validation Failed
- Log Error - SSF Request Failed
- Assign - Calculate Total Amount

Avoid:

- Assign
- Log Message
- Throw
- Sequence1

### Important Rule

Activity names should reflect actual behavior.

Avoid misleading names.

Bad example:

- Log Success - Output Generated

inside a System Exception catch flow.

### Why This Matters

Activity names appear during:

- Production Debugging
- Support Investigation
- Screenshot-Based Reviews
- Runtime Traces
- Incident Analysis

Meaningful names improve operational readability significantly.

## Review Guidance

Check:

- Are Workflow Names Descriptive?
- Are Variables Named Consistently?
- Are Argument Directions Correct?
- Are Activity Names Meaningful?
- Do Names Match Actual Behavior?
- Are Generic Names Avoided?

Flag as Suggestion if:

- Naming Can Be Improved Slightly

Flag as Warning if:

- Naming Reduces Readability/Supportability
- Activity Names Are Misleading
- Argument Directions Are Incorrect

Flag as Critical only if:

- Misleading Naming Causes Operational Misinterpretation
- Incorrect Argument Naming Hides Mutation/Side Effects
- Workflow Meaning Becomes Unclear During Support/Debugging
