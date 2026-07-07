# Session Workflow Playbook

## Purpose

This playbook converts the study's capability findings into reusable operational guidance for starting real projects in new ChatGPT sessions.

It is not a system prompt and it is not a project-specific instruction file. Its role is broader:

```text
project request
        ↓
capability-aware assessment
        ↓
workflow topology selection
        ↓
project-specific execution plan
        ↓
reusable recipes and workarounds as needed
```

The central question is not:

```text
How do I force this project to run inside the session sandbox?
```

It is:

```text
Where should each part of this project run,
and how should source, execution, persistence, and publication be composed?
```

## Relationship to instruction files and skills

This playbook is conceptually related to repository instruction files and reusable agent skills, but it serves a different layer.

```text
system / session instruction
  broad behavioral contract

project instruction file
  repository-specific conventions and standing rules

skill / recipe
  repeatable task-specific procedure

Session Workflow Playbook
  capability map + topology selection + tradeoffs + failure modes + recipes
```

The playbook may later generate or inform project-local `AGENTS.md`, `CLAUDE.md`, skills, prompts, scripts, or checklists. It should not assume that any one agent product or instruction mechanism is always present.

For the detailed architecture, alternatives considered, design rationale, and external references, see [`DESIGN_RATIONALE.md`](DESIGN_RATIONALE.md).

## Design principles

1. **Possibility, not doctrine.** The study established what can be done and under what conditions. It did not prove that the session sandbox is always the optimal execution location.
2. **Topology before tooling.** Decide where code, compute, durable state, large artifacts, and publication should live before substantial work begins.
3. **Status-qualified guidance.** Every recommendation should distinguish verified capability, conditional verification, environment sensitivity, plausible but untested paths, and capabilities not observed.
4. **Tradeoffs are first-class.** Present alternatives with costs, constraints, and failure modes rather than a single universal workflow.
5. **Do not repeat capability discovery unnecessarily.** Revalidate current tool surfaces and volatile environment facts, but do not repeat full experiments when prior evidence already establishes the relevant method.
6. **Observation and inference remain separate.** Operational guidance may recommend a choice while still labeling which parts are directly verified and which are inferred or untested.
7. **Durability is explicit.** Runtime success does not imply artifact persistence. Checkpoint and off-runtime preservation should be designed before long or valuable execution.
8. **Publication is separate from preservation.** Immediate durable backup and curated public Release publication are different stages with different constraints.

## Status vocabulary

| Status | Meaning |
|---|---|
| `VERIFIED` | End-to-end capability directly demonstrated in the study. |
| `VERIFIED_WITH_CONDITIONS` | Demonstrated under specific runtime, dependency, workload, or connector conditions. |
| `ENVIRONMENT_SENSITIVE` | Method is usable but tool surface, mirrors, runtime state, or provider behavior should be rechecked. |
| `PLAUSIBLE_NOT_TESTED` | Architecturally reasonable path identified but not validated end-to-end. |
| `NOT_OBSERVED` | Capability was not demonstrated in the study. This does not prove impossibility. |

## Current playbook artifacts

- `CHATGPT_SESSION_BOOTSTRAP.md` — explicit entry brief for a fresh ChatGPT project session.
- `CAPABILITY_MATRIX.tsv` — compact map of options, status, conditions, tradeoffs, known failures, and workarounds.
- `WORKFLOW_DECISION_TREE.md` — project-topology selection process.
- `PRACTICAL_GUIDE_OUTLINE.md` — structure for the full operational guide and reusable recipes.
- `DESIGN_RATIONALE.md` — design philosophy, alternatives considered, architecture rationale, and reference material.
- `TODO.md` — deferred implementation, validation, recipe, adapter, and future experiment work.

## Intended use in a new ChatGPT project session

A new session should use the playbook as prior operational knowledge, then:

```text
1. characterize the new project
2. recheck volatile capabilities relevant to the task
3. choose an execution and persistence topology
4. establish source control and checkpoint strategy
5. select only the recipes required for the project
6. execute work without repeating already-solved capability experiments
7. preserve new project-specific findings separately from this general playbook
```

The playbook is therefore best understood as a **session workflow selection and execution layer**, not merely as onboarding documentation.

## Current development state

The foundation phase is complete and intentionally paused before recipe implementation.

When work resumes, follow [`TODO.md`](TODO.md) rather than expanding the guide indiscriminately. The next phase should prioritize a small set of evidence-backed procedures and test them in a genuinely new real project session.
