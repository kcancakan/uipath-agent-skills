# Standard REFramework Contract

## Purpose

This document defines accepted runtime and operational behaviors for standard UiPath REFramework-based projects.

The purpose of this contract is to prevent false-positive findings caused by applying generic software engineering assumptions directly to UiPath REFramework implementations.

This document describes:

- Expected framework responsibilities
- Accepted operational patterns
- Runtime caveats
- Framework-owned behaviors
- Review boundaries

---

# Framework Ownership

In standard REFramework projects, the framework owns:

- Transaction lifecycle
- Retry handling
- Queue retry orchestration
- Transaction status transitions
- Exception routing
- Screenshot handling
- Framework logging
- Framework initialization lifecycle
- Framework cleanup lifecycle
- Config loading lifecycle
- SetTransactionStatus behavior
- RetryNumber handling
- Framework-level Orchestrator interactions

Process workflows are expected to contain only:

- Business logic
- Transaction-specific validation
- Process-specific logging
- Transaction output preparation

---

# Accepted Framework Behaviors

## BusinessRuleException On Empty Queue

Throwing:

```vb
New BusinessRuleException("No Data")

when no transaction exists is an accepted REFramework behavior.

Do not classify this as a defect.

Framework-Level Retry Handling

Retry behavior is controlled by:

Config
Queue configuration
RetryNumber
SetTransactionStatus
Framework transaction state handling

Process.xaml is not expected to implement custom retry orchestration unless explicitly customized.

Framework Logging Ownership

REFramework already produces framework-level logs for:

Transaction start
Transaction end
Init start/end
Process start/end
Exception lifecycle

Do not classify missing manual logs as Critical if framework logging already exists.

Transaction Status Lifecycle

Framework controls transaction completion semantics through:

SetTransactionStatus.xaml
Queue transaction result handling
Exception classification

Do not assume every BusinessRuleException automatically means failure from an operational perspective.

BusinessRuleException may intentionally represent:

Duplicate transaction
Already processed transaction
Business rejection
Validation rejection
Skipped transaction

Operational meaning depends on process design and PDD.

Config-Based Resource Management

The following are accepted REFramework patterns:

Config.xlsx usage
Orchestrator Assets usage
String replacement inside connection strings
Environment-dependent config values
Config dictionary access from workflows

Do not classify Config usage itself as a defect.

Workflow Invocation Patterns

Invoke Workflow File is an accepted orchestration mechanism.

Large orchestrator workflows coordinating multiple smaller workflows are expected.

Do not classify orchestration-style workflows as violating Single Responsibility by default.

Framework Exception Routing

Standard REFramework separates:

BusinessRuleException
System.Exception

through framework-level TryCatch structures.

However:

UiPath runtime semantics may differ from standard .NET catch ordering assumptions.

Do not assume:

Generic Exception Catch
    before
BusinessRuleException Catch

always shadows BRE routing.

Runtime validation may be required.

Queue Retry Ownership

Queue retry lifecycle is framework-owned.

Do not classify queue retry handling as missing unless:

Retry configuration is clearly broken
Queue retry behavior contradicts business expectations
Runtime evidence proves incorrect retry semantics
Output/Report Ownership

Framework typically owns:

Transaction reporting lifecycle
Output row creation
Exception output formatting
Retry metadata handling
Transaction summary generation

Process.xaml is expected to prepare transaction-specific values only.

Runtime Caveats
Invoke Code Exception Wrapping

Invoke Code may wrap exceptions before propagating them to outer TryCatch activities.

BusinessRuleException thrown inside Invoke Code may not be caught directly as BRE outside.

Do not assume Invoke Code preserves exception types exactly.

Reviewers should verify:

Explicit result/flag pattern
Safe InnerException handling
Runtime-tested BRE routing
Reference-Type Runtime Behavior

DataTable, Dictionary, JObject, List and similar reference types may mutate outer state even when passed as in_ arguments.

Direction alone does not guarantee immutability.

Transitive Package Resolution

UiPath projects may successfully execute even if some dependencies are resolved transitively rather than explicitly listed in project.json.

Do not assume missing package references automatically cause runtime failure.

False-Positive Prevention Rules

Do not automatically classify the following as Critical:

Manual logs
Generic Exception before BRE
Config-based credentials
Config dictionary usage
Invoke Workflow orchestration style
Large Process.xaml orchestration flows
Framework-level retries
Queue retry ownership
Framework output handling
Transaction status semantics
Transitive package references

unless direct operational evidence exists.

Manual Validation Areas

The following areas often require:

Runtime validation
PDD validation
Orchestrator validation
Production evidence

before high-severity classification:

BRE vs SystemException semantics
Queue retry correctness
Reconciliation semantics
Transaction completion meaning
Business rejection meaning
Duplicate transaction behavior
Reporting lifecycle consistency
SetTransactionStatus behavior
Queue retry expectations
Invoke Code BRE propagation
Severity Guidance
Critical

Use only when:

Direct operational corruption risk exists
Reconciliation inconsistency exists
Retry semantics are proven incorrect
Runtime failure evidence exists
Silent data corruption is possible
Queue/report/DB state divergence exists
Warning

Use when:

Maintainability risk exists
Runtime ambiguity exists
Supportability risk exists
Framework contract may be violated
Runtime behavior is suspicious but unproven
Suggestion

Use when:

Improvement opportunity exists
Naming/readability issue exists
Minor maintainability issue exists
No direct operational consequence exists
Reviewer Reminder

Standard REFramework projects should be reviewed primarily from:

Operational correctness
Runtime semantics
Supportability
Transaction integrity
Reporting consistency
Queue lifecycle safety

not from generic software engineering purity alone.