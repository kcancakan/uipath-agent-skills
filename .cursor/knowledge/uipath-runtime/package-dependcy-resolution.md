# Package Dependency Resolution

## Generic Assumption

If a package dependency is not explicitly listed in `project.json`, many static reviewers and AI models assume the project will fail during build or runtime.

This assumption is often based on traditional dependency-management expectations from standard .NET ecosystems.

## Actual UiPath Behavior

UiPath package resolution may automatically restore indirect/transitive dependencies during runtime, Studio restore operations, or package installation flows.

This means:

- an activity may work correctly
- a dependency may resolve successfully
- the process may run without issue

even if the dependency is not explicitly visible inside the project's direct dependency list.

This behavior is especially common when:

- a parent package already references the dependency
- the dependency is restored from UiPath feeds
- Studio/runtime automatically resolves package trees
- internal/company feeds contain transitive package mappings

## Why This Matters

AI/static reviewers may incorrectly classify indirect dependency usage as:

- broken build
- CI/CD failure
- runtime failure
- missing package error

without actual evidence.

This creates noisy findings and false-positive infrastructure risks.

## Review Guidance

Do not classify indirect/transitive dependency usage as Critical unless there is direct evidence such as:

- failed restore logs
- runtime package load failure
- missing activity errors
- Workflow Analyzer/package resolution errors
- CI/CD restore failures

Prefer:

- Manual Check
- Warning
- portability/reproducibility concern

instead of direct build/runtime-failure assumptions.

## Example Scenario

A project uses an activity from:

`BalaReva.Excel`

The package is not explicitly listed in:

`project.json`

However:

- the activity works correctly
- Studio resolves the dependency automatically
- runtime execution succeeds
- CI/CD pipeline restores packages successfully

In this case, the issue is not necessarily a runtime failure.

The actual concern is more likely:

- dependency visibility
- clean-machine reproducibility
- onboarding clarity
- explicit dependency governance

rather than an immediate operational failure.

## Preferred Interpretation

Instead of:

Critical: Missing dependency will break runtime

Prefer:

Warning: Dependency appears to be resolved transitively.
Validate reproducibility in clean environments and CI/CD pipelines.
