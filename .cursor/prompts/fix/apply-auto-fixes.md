Auto-Fix Execution Prompt

# PHASE 1 — Knowledge and Fix Context Initialization

Before applying any fixes:

1. Read and internalize:
   - .cursor/knowledge/**
   - .cursor/rules/fix/**
   - .review/rpa-review-result.md
   - .review/rpa-fix-readiness.md

2. Treat the knowledge layer as the operational source of truth.

3. Treat `.review/rpa-fix-readiness.md` as the authoritative fix classification source.

Do NOT independently reclassify findings.

Only apply findings explicitly classified as:

- Auto-fixable

Do not apply:
- Semi-auto-fixable findings
- Manual-only findings
- Findings without classification

---

# PHASE 2 — Fix Planning

Before editing files:

1. Identify:
   - Finding ID
   - Affected files
   - Affected workflows
   - Operational risk areas
   - Framework-owned areas

2. Validate that:
   - the finding still exists
   - the repository state still matches the finding evidence
   - the planned edit is localized and deterministic

3. Build a fix plan.

The fix plan must include:
- Finding ID
- Planned file changes
- Why the change is considered safe
- Potential validation needs

Do not edit files yet during planning.

---

# PHASE 3 — Auto-Fix Execution

Apply only:
- minimal
- localized
- reviewable
- deterministic

patches.

Preserve:
- business logic
- queue lifecycle
- retry semantics
- reporting lifecycle
- framework ownership
- transaction integrity
- exception routing

Avoid:
- broad refactors
- formatting-only rewrites
- unrelated cleanup
- opportunistic improvements

Do not modify unrelated sections of workflows.

---

# UiPath Workflow Safety Rules

When editing XAML workflows:

- preserve activity order
- preserve TryCatch boundaries
- preserve StateMachine routing
- preserve argument mappings
- preserve workflow invocation integrity

Do not:
- rename invoked workflows blindly
- rename arguments blindly
- change argument direction without reference validation
- move variables across TryCatch boundaries blindly
- modify queue lifecycle behavior
- modify reporting lifecycle behavior
- modify framework-owned flows

If fix safety becomes unclear:
- stop
- skip the finding
- document why

---

# Safety Brake

Even if a finding is marked Auto-fixable:

SKIP the fix if the actual edit appears to affect:
- business logic
- queue semantics
- retry behavior
- reporting lifecycle
- exception lifecycle
- external system behavior
- framework lifecycle ownership

Document skipped findings clearly.

---

# PHASE 4 — Fix Reporting

After applying fixes:

Write:

.review/rpa-fix-result.md

The report must contain:

# Applied Fixes
- Finding ID
- Modified File
- Applied Change
- Why Safe

# Skipped Fixes
- Finding ID
- Skip Reason
- Detected Operational Risk

# Validation Notes
- Runtime validation needs
- Manual verification needs
- Potential regression areas

# Modified Files Summary
- Full modified file list

---

# Output Rules

After completing the fixes, reply in chat with only:

Fix result file path
Applied fix count
Skipped fix count
Modified file count
Manual validation required