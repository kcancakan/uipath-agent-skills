# UiPath Agent Skills

> Operationally-aware AI review skills for UiPath RPA projects.

This repository experiments with adding:
- review rules
- operational knowledge
- framework semantics
- supportability heuristics
- structured review artifacts

on top of AI-assisted code review workflows.

**The primary goal is not generating more findings.**  
**The primary goal is generating more operationally meaningful findings for enterprise-grade UiPath automations.**

---

## Motivation

Most frontier AI models are already quite strong at:
- generic software engineering review
- static analysis
- naming consistency
- configuration/security findings
- dependency/package checks

However, real-world RPA projects usually contain additional operational complexity such as:
- transaction lifecycle management
- queue orchestration
- reporting/output contracts
- framework sequencing
- retry/idempotency concerns
- operational logging
- supportability expectations
- runtime-specific behavior

Many of these issues are technically valid flows but operationally problematic. This repository experiments with whether an additional “skill layer” can improve review quality beyond generic coding-model behavior.

---

## Current Focus Areas

The current rules and prompts focus mainly on:
- UiPath REFramework/Rocky-style processes
- transactional automation patterns
- queue handling
- logging quality
- exception handling
- supportability
- maintainability
- CI/CD hygiene
- config/security handling
- operational risk detection
- review artifact generation
- fix-agent compatibility

---

## Repository Structure

```text
.
├── .cursor/
│   ├── rules/       # Modular review rules (naming, logging, exceptions, config, etc.)
│   └── prompts/     # Reusable review prompts (full review, focused review, fix-readiness)
├── .review/         # Generated review artifacts (e.g., rpa-review-result.md)
├── examples/        # Sample review outputs and example findings
└── knowledge/       # Experimental operational knowledge (historical patterns, heuristics)
```

### Detailed Breakdown

- **`.cursor/rules`**: Contains modular review rules such as naming standards, workflow standards, logging rules, exception/queue handling, config/security checks, evidence-based severity rules, review artifact formatting.
- **`.cursor/prompts`**: Contains reusable review prompts such as full project review, focused review, and fix-readiness review.
- **`.review`**: Generated review artifacts are written here. Example: `.review/rpa-review-result.md`.
- **`knowledge`**: Experimental operational knowledge layer intended for historical failure patterns, runtime semantics, supportability heuristics, operational review examples, and framework-specific contextual knowledge.
- **`examples`**: Sample review outputs and example findings.

---

## Example Review Philosophy

The repository intentionally separates findings into two categories:

### 1. Generic SWE findings
*Examples:* hardcoded credentials, naming inconsistencies, dependency hygiene, weak logging, static configuration risks.

### 2. Operational RPA findings
*Examples:* output/report corruption risks, retry/idempotency side effects, framework lifecycle issues, transaction visibility gaps, contextual maintainability problems, supportability risks, queue/reporting edge cases.

### Important Observation

One of the main observations during experimentation:
Frontier models are usually good at syntax review, static patterns, and generic SWE best practices. But they still struggle with:
- UiPath runtime semantics
- operational consequences
- framework behavior
- historical production failure patterns
- support-engineer style reasoning

This repository experiments with whether structured rules, operational knowledge, review contracts, and contextual heuristics can improve AI-assisted review quality.

---

## What This Repository Is NOT

This repository is **NOT**:
- an official UiPath project
- a replacement for human code review
- a guarantee of production safety
- a static analyzer replacement
- a production-ready governance platform

The repository is currently experimental and intended for learning/research purposes around operationally-aware AI review workflows.

---

## Local / On-Prem Possibility

The repository is intentionally designed so that the review flow does not have to depend only on cloud-hosted models. Possible setups include:
- Cursor + cloud LLMs
- Cursor + local models
- Ollama-based local review pipelines
- LM Studio-based local review flows
- enterprise-hosted model gateways

The long-term idea is making the review layer portable across different providers and environments.

---

## Future Ideas

Potential future directions:
- operational knowledge retrieval
- runtime-aware review context
- structured review datasets
- fix-agent workflows
- review-to-patch generation
- hybrid static-analysis + LLM review
- local/on-prem review orchestration
- RPA-specific review benchmarking

---

## Disclaimer

⚠️ **The findings produced by AI-assisted review systems should always be validated by experienced engineers.**

Especially for:
- retry semantics
- business exception handling
- queue lifecycle behavior
- runtime-specific behavior
- infrastructure integrations
- production support implications

Human operational review remains critical.

---

## Status

**Experimental / early-stage.**

Currently evolving through:
- real project reviews
- multi-model comparisons
- operational finding analysis
- iterative rule improvements
- contextual knowledge experiments