# Experiment 002 — Knowledge-Layer-Assisted Operational RPA Review

## Goal

Evaluate whether adding a knowledge/runtime layer improves operationally meaningful UiPath project reviews beyond prompt/rule-based review approaches.

## Setup

### Framework

Customized REFramework-based architecture with operational reporting lifecycle and custom queue/output handling.

### Process Characteristics

- Queue-based performer process
- Dynamic invoice/ruleset classification
- DB update and reconciliation logic
- JSON parsing and value extraction
- API/File/DB interaction
- Output reporting lifecycle
- Different business logic flows
- Cross-workflow operational dependencies

### Models

- GPT-5.5
- Sonnet 4.6
- Gemini 3.1
- Composer 2.5

### Shared Context

| Aspect | Detail |
|--------|--------|
| Prompts | Same |
| Repository | Same |
| Review artifact structure | Same |
| Knowledge layer | Same |
| Operational rules | Same |
| Framework contracts | Same |
| Approximate tokens | ~128k–250k |

### Knowledge Layer Included

- UiPath runtime semantics
- Framework lifecycle contracts (custom)
- Output/reporting behaviour
- Development standards
- Runtime caveats and accepted framework behaviours

---

## Findings Matrix

| Finding / Revision Item | Description | GPT-5.5 | Sonnet 4.6 | Gemini 3.1 | Composer 2.5 |
|-------------------------|-------------|:-------:|:----------:|:----------:|:--------------:|
| Hardcoded passwords in CI/CD YAML | Static deployment credentials committed in repository | ✅ | ❌ | ❌ | ✅ |
| Invoke Code exception swallowing | Exceptions caught in try/catch but not rethrown; only logged inside Invoke Code | ✅ | ✅ | ✅ | ✅ |
| Missing try/catch in Invoke Code | An Invoke Code with LINQ had no internal try/catch | ✅ | ✅ | ✅ | ✅ |
| Output variable initialisation risk | Output variables used in catch paths without earlier safe assignment | ✅ | ✅ | ✅ | ✅ |
| Framework assignment logic bug | Queue override argument assigned incorrectly → logic bug | ✅ | ❌ | ❌ | ✅ |
| BRE / DB reconciliation inconsistency | Some BRE scenarios expected different DB statuses per analysis; not all should be `OK` | ❌ | ✅ | ❌ | ❌ |
| Silent stale ruleset match risk | Invoke Code bug may yield empty/stale dictionary; flow continues | ❌ | ✅ | ❌ | ❌ |
| Manual workflow lifecycle logs | Manual start/end logs instead of invocation settings | ✅ | ✅ | ✅ | ✅ |
| File/class naming inconsistency | `x:Class` and workflow file names do not match | ❌ | ✅ | ❌ | ❌ |
| Wrong activity naming | Update-query activity labelled as Select | ❌ | ✅ | ❌ | ❌ |
| Unused variable detection | Unused variables left in workflow | ❌ | ✅ | ❌ | ❌ |
| Contextual unused variable detection | Variable only used in a trivial Assign | ✅ | ✅ | ❌ | ❌ |
| TODO / development artifact detection | Incorrect placeholder comment left in workflow | ❌ | ✅ | ❌ | ❌ |
| Duplicate logic detection | Separate Invoke Code instead of reusable pattern | ❌ | ✅ | ❌ | ❌ |
| Reusable variable optimisation | Same assignment repeated across Init, Process Try, TryCatch | ❌ | ✅ | ❌ | ❌ |
| Runtime JSON null safety risk | `JArray` / `SelectToken` access without null-safe validation | ✅ | ✅ | ❌ | ✅ |
| Unverified file reading | File read / Base64 without existence check | ❌ | ✅ | ❌ | ❌ |
| Incorrect argument direction | `io_` used for read-only connection | ✅ | ❌ | ✅ | ✅ |
| Same variable for different arguments | Same value passed to two semantically different arguments | ❌ | ✅ | ❌ | ❌ |
| Exception handling interpretation* | BRE/`System.Exception` catch order interpreted cautiously vs generic .NET | ⚠️ | ⚠️ | ⚠️ | ✅ |
| Operational reconciliation reasoning | Cross-flow DB/report/queue consistency | ✅ | ✅ | ❌ | ❌ |
| Framework contract awareness | Framework-owned behaviours correctly excluded | ✅ | ✅ | ✅ | ❌ |
| **Tokens used during review** | Approximate total review context | ~165k | ~250k | ~135k | ~128k |

\* Although classification levels are better now, models still sometimes point to catch order as an issue except Composer 2.5.

---

## Most Common Success Pattern

All models improved significantly on:

- Framework-aware interpretation
- Runtime-oriented reasoning
- False-positive reduction
- Operational review quality
- Supportability-focused findings
- Queue/reporting lifecycle understanding

---

## Most Interesting Improvement

The interesting part was **not** increasing finding count.

It was changing **how** the models interpreted the same workflow after operational/runtime knowledge was introduced.

---

## Most Interesting Finding

**Anthropic Sonnet 4.6** surfaced an inconsistency in `BusinessRuleException` handling: different BRE scenarios were expected to produce different DB statuses (`OK` vs `NOK`), but the process updated all cases as `OK` without conditional validation.

The valuable aspect was inference depth — not only flagging a mismatch, but inferring that the process lacked a **decision layer** distinguishing BRE outcomes, risking operational reconciliation gaps between queue results and DB state.

This drew on:

- Cross-workflow reasoning
- Exception lifecycle understanding
- DB/report reconciliation awareness
- Analysis document knowledge

---

## Most Interesting False-Positive Reduction

**BRE / `System.Exception` catch-order interpretation.**

In Experiment 001, models applied generic .NET assumptions aggressively.

After runtime semantics knowledge was introduced, models became more cautious and treated this as:

- Runtime-dependent behaviour
- Manual validation area
- Framework/runtime caveat

…instead of automatically classifying it as a critical defect.

---

## Additional Observation — Workflow Analyzer Artifact Usage

**GPT-5.5** and **Composer 2.5** picked up historical Workflow Analyzer results under:

- `.local/.analysis` (forgotten from studio run)

Rather than turning those outputs directly into production findings, models framed them as:

- Contextual review signals
- Historical analyzer artifacts
- Manual validation candidates before production deployment

This extended review beyond workflows to:

- Repository metadata
- Historical tooling artifacts
- Local analyzer traces
- Development-environment leftovers  

…as operational review context.

---

## Current Observation

Prompt/rule scaling alone was insufficient for operationally meaningful RPA review quality.

The main uplift came from:

- Runtime semantics
- Framework contracts
- Operational lifecycle understanding
- Expected development standards
- Accepted framework behaviour knowledge

---

## Current Limitation

Cursor does **not** automatically apply prompts under `.cursor`.

Even when review/fix prompts exist under:

- `.cursor/prompts/review`
- `.cursor/prompts/fix`

**explicit invocation** of those prompts is still required during execution.

---

## Auto-Fix Observation

A small **guarded auto-fix** experiment was run with GPT-5.5 as a mid-level performer model.

Observations:

- Deterministic/mechanical fixes are becoming reliable
- Naming/logging/style fixes mostly succeeded
- Output initialisation and small runtime-safe patches worked well
- Safety-brake behaviour worked surprisingly well

In several cases the model **skipped** technically auto-fixable findings when it detected risk around:

- Runtime semantics
- Framework lifecycle
- Reporting lifecycle
- Transaction flow behaviour  

So the model began reasoning about **when not** to fix, not only how to fix.

---

## Knowledge Layer Limitation

The current knowledge layer remains relatively thin. It mostly covers:

- Runtime semantics
- Framework contracts
- Operational caveats
- Development standards

**Possible expansions:**

- Support investigation patterns
- Production incident patterns
- Retry/reconciliation heuristics
- Historical failure scenarios
- Operational troubleshooting playbooks
- Support-team behavioural knowledge

---

## Next Experiment

### Experiment 003 — Guarded Auto-Fix & Re-Review Loop

**Planned flow:**

1. Operational review  
2. Fix-readiness classification  
3. Guarded auto-fix  
4. Re-review validation  
5. Regression/safety evaluation  

**Focus areas:**

- Deterministic remediation
- Runtime-safe patch generation
- Framework-aware edits
- Semantic regression prevention
- Operational safety boundaries
