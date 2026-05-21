# BusinessRuleException Catch Order

## Generic Assumption

In traditional C#/.NET applications, placing a generic Exception catch before a specific exception type may shadow the specific catch block.

Many frontier models incorrectly transfer this assumption directly into UiPath workflow reviews.

## Actual UiPath Behavior

UiPath TryCatch runtime behavior may still correctly resolve BusinessRuleException catches even when a generic System.Exception catch appears earlier in XAML structure.

Static XAML ordering alone is not enough evidence to classify this as a runtime issue.

## Why This Matters

Multiple AI review models may incorrectly classify this pattern as:
- Critical
- Retry risk
- Incorrect exception handling

even though actual runtime execution behaves correctly.

This creates false positives and reduces review quality.

## Review Guidance

Do not classify BusinessRuleException/System.Exception catch ordering as Critical without:
- Runtime validation
- Workflow Analyzer evidence
- Observed retry/status issues
- Proven exception shadowing behavior

Prefer:
- Manual Check
- Suggestion
- Low-confidence Warning

instead of direct Critical classification.

## Example Scenario

A workflow:
- Throws BusinessRuleException inside Invoke Code
- Contains both BusinessRuleException and System.Exception catches
- Appears to catch generic Exception first in XAML ordering

Models may incorrectly assume:
BusinessRuleException will always be swallowed by generic Exception handling.

Actual UiPath runtime behavior may differ.