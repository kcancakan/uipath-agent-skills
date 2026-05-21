# UiPath Agent Skills

Operationally-aware AI review skills for UiPath RPA projects.

This repository experiments with adding:

- Review Rules
- Operational Knowledge
- Framework Semantics
- Supportability Heuristics
- Structured Review Artifacts

on top of AI-assisted code review workflows.

The primary goal is not generating more findings.

The primary goal is generating more operationally meaningful findings for enterprise-grade UiPath automations.

---

# Motivation

Most frontier AI models are already quite strong at:

- Generic Software Engineering Review
- Static Analysis
- Naming Consistency
- Configuration/Security Findings
- Dependency/Package Checks

However, real-world RPA projects usually contain additional operational complexity such as:

- Transaction Lifecycle Management
- Queue Orchestration
- Reporting/Output Contracts
- Framework Sequencing
- Retry/Idempotency Concerns
- Operational Logging
- Supportability Expectations
- Runtime-Specific Behavior

Many of these issues are technically valid flows but operationally problematic.

This repository experiments with whether an additional “skill layer” can improve review quality beyond generic coding-model behavior.

---

# Current Primary Focus Areas

- Runtime-Aware Review
- Framework-Aware Interpretation
- Operational Supportability Reasoning
- False-Positive Reduction
- Guarded Remediation Safety
- UiPath REFramework/Rocky-Style Processes
- Transactional Automation Patterns
- Queue Handling
- Logging Quality
- Exception Handling
- Supportability
- Maintainability
- CI/CD Hygiene
- Config/Security Handling
- Operational Risk Detection
- Review Artifact Generation
- Fix-Agent Compatibility

---

# Repository Structure

```text
.cursor/
├── knowledge/
│   ├── development-standards/
│   ├── framework-behaviours/
│   └── uipath-runtime/
│
├── prompts/
│   ├── fix/
│   └── review/
│
└── rules/
    ├── fix/
    └── review/

experiments/
sample-result/
```

---

# Detailed Breakdown

`.cursor/knowledge`

Contains runtime semantics, framework contracts, operational caveats, development standards, and false-positive prevention guidance for UiPath projects.

`.cursor/rules`

Contains modular review/fix behavioral rules such as logging standards, exception handling expectations, queue/retry safety, naming conventions, severity guidance, and operational review boundaries.

`.cursor/prompts`

Contains reusable operational review/fix prompts such as full review, focused review, fix-readiness analysis, and guarded auto-fix flows.

`sample-result`

Contains anonymized real-world review outputs demonstrating operational reasoning, framework-aware findings, and runtime-oriented analysis quality.

`experiments`

Contains benchmark-style experiment results comparing different models, knowledge-layer impact, and operational review behavior evolution.

---

# Example Review Philosophy

The repository intentionally separates findings into two categories:

## 1. Generic SWE Findings

Examples:

- Hardcoded Credentials
- Naming Inconsistencies
- Dependency Hygiene
- Weak Logging
- Static Configuration Risks

## 2. Operational RPA Findings

Examples:

- Output/Report Corruption Risks
- Retry/Idempotency Side Effects
- Framework Lifecycle Issues
- Transaction Visibility Gaps
- Contextual Maintainability Problems
- Supportability Risks
- Queue/Reporting Edge Cases

---

# Important Observation

One of the main observations during experimentation:

Frontier models are usually good at:

- Syntax Review
- Static Patterns
- Generic SWE Best Practices

But they still struggle with:

- UiPath Runtime Semantics
- Operational Consequences
- Framework Behavior
- Historical Production Failure Patterns
- Support-Engineer-Style Reasoning

This repository experiments with whether structured rules, operational knowledge, review contracts, and contextual heuristics can improve AI-assisted review quality.

One of the most interesting observations was:

The major improvement did not come from increasing prompt size.

It came from changing how the models interpreted the same workflow after runtime semantics and framework knowledge were introduced.

---

# What This Repository Is NOT

This repository is NOT:

- An Official UiPath Project
- A Replacement For Human Code Review
- A Guarantee Of Production Safety
- A Static Analyzer Replacement
- A Production-Ready Governance Platform

The repository is currently experimental and intended for learning/research purposes around operationally-aware AI review workflows.

---

# Local / On-Prem Possibility

The repository is intentionally designed so that the review flow does not have to depend only on cloud-hosted models.

Possible setups include:

- Cursor + Cloud LLMs
- Cursor + Local Models
- Ollama-Based Local Review Pipelines
- LM Studio-Based Local Review Flows
- Enterprise-Hosted Model Gateways

The long-term idea is making the review layer portable across different providers and environments.

---

# Future Ideas

Potential future directions:

- Operational Knowledge Retrieval
- Runtime-Aware Review Context
- Structured Review Datasets
- Fix-Agent Workflows
- Review-To-Patch Generation
- Hybrid Static-Analysis + LLM Review
- Local/On-Prem Review Orchestration
- RPA-Specific Review Benchmarking
- Guarded Auto-Fix Flows
- Re-Review Validation Loops
- Runtime-Safe Remediation
- Operational Regression Prevention
- Framework-Aware Fix Strategies

---

# Disclaimer

⚠️ The findings produced by AI-assisted review systems should always be validated by experienced engineers.

Especially for:

- Retry Semantics
- Business Exception Handling
- Queue Lifecycle Behavior
- Runtime-Specific Behavior
- Infrastructure Integrations
- Production Support Implications

Human operational review remains critical.

---

# Status

Experimental / research-oriented.

Currently evolving through:

- Real Project Reviews
- Multi-Model Comparisons
- Operational Finding Analysis
- Iterative Rule Improvements
- Contextual Knowledge Experiments