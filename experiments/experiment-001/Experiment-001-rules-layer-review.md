# Experiment 001 — Multi-Model Operational RPA Review

## Goal

Evaluate whether frontier models can perform operationally meaningful reviews for UiPath projects using a custom skill ruleset.

## Setup

### Framework
Customized REFramework-based architecture

### Process Flow
- Set file permissions to viewer
- Read excel input file
- Add items to queue
- Calculate total amount based on input *  given vat rate
- Process API requests with the calculated data
- Add transaction to output datatable
- Finish all transactions
- Update input file with results
- Set file permissions to editor
- Send output mail

### Models
- GPT-5.5
- Sonnet 4.6
- Gemini 3.1

### Shared Context
- Same prompts
- Same rules
- Same repository
- Same review artifact structure
- ~110k–125k tokens

## Findings Matrix

| Finding / Revision Item | Description | GPT-5.5 | Sonnet 4.6 | Gemini 3.1 |
| :--- | :--- | :--- | :--- | :--- |
| Hardcoded passwords in CI/CD yaml | Static deployment credentials committed in repository | ✅ | ✅ | ✅ |
| String/SecureString handling | Plain string password usage instead of SecureString-only flow | ✅ | ✅ | ✅ |
| Static API URL usage | Environment-specific API URLs stored statically | ✅ | ✅ | ✅ |
| ContinueOnError usage | Retry Scope configured with ContinueOnError=True | ✅ | ✅ | ✅ |
| Non-unique queue reference usage | Queue reference may not be unique according to process logic | ✅ | ✅ | ✅ |
| Missing operational logging | Some flows missing meaningful start/end logs | ✅ | ✅ | ✅ |
| Weak logging context | Logs contain generic information instead of transaction-specific details | ✅ | ✅ | ✅ |
| File/Class naming inconsistency | Duplicate or inconsistent x:Class naming | ✅ | ✅ | ✅ |
| Document identification | Wrong ReadMe added to project alongside analyse file | ✅ | ✅ | ✅ |
| Unused variable | Explicit unused variable detected | ❌ | ✅ | ❌ |
| Contextual unused variable | Variable only used in trivial assign/pass-through operations | ❌ | ❌ | ❌ |
| Wrong process naming format | Project naming convention mismatch | ✅ | ✅ | ✅ |
| If-Else usage pattern | Main logic placed under Else instead of Then | ✅ | ✅ | ✅ |
| Unnecessary activity usage | File permission/view operation repeated unnecessarily | ❌ | ❌ | ❌ |
| Potential null reference risk | Output/report structures may receive null values | ❌ | ❌ | ❌ |
| Wrong activity naming | Activity names misleading for actual flow purpose | ❌ | ❌ | ❌ |
| Switch-case duplication | Case and Default branches contain same logic | ✅ | ✅ | ❌ |
| Wrong sheet name usage | Excel update step targets incorrect sheet name | ❌ | ❌ | ❌ |
| Template usage optimization | Request template re-read every transaction instead of Init phase | ❌ | ✅ | ❌ |
| Naming rule violations | Variables/activities partially violate naming standards | ✅ | ✅ | ✅ |
| Exception handling interpretation* | Models flagged BRE/System.Exception catch order based on generic .NET assumptions, but this is not necessarily a UiPath runtime issue | ⚠️ | ⚠️ | ⚠️ |
| Package Dependicies* | BalaRava excel package used in inner activity but not directly added to dependicies | ❌ | ⚠️ | ❌ |
| Tokens used during review | | ~125k | ~110k | ~115k |

*UiPath runtime behave differently from those general C#/.net rules

### Most Common Success Pattern

All models performed strongly on:
- security findings
- static analysis
- generic SWE-style governance checks

### Weak Areas
- UiPath runtime semantics
- Operational consequences
- Framework lifecycle behavior
- Contextual workflow meaning
- Supportability-oriented reasoning

## Most Interesting False Positive

BRE/SystemException catch-order interpretation.

All models applied generic .NET assumptions, while actual UiPath runtime behavior behaved differently.

## Current Hypothesis

Prompt/rules alone may not be enough for operationally meaningful RPA review.

Potential missing layer:
- Runtime semantics
- Operational knowledge
- Historical failure patterns
- Supportability heuristics

## Next Experiment

Knowledge-layer-assisted review flow. Knowledge layer will consist of experiences of a developer/support.