# Session Workflow Playbook — TODO

## Current pause point

The playbook foundation has been designed and documented. This is an intentional stopping point before recipe implementation.

Completed foundation:

```text
playbook/README.md
playbook/CHATGPT_SESSION_BOOTSTRAP.md
playbook/CAPABILITY_MATRIX.tsv
playbook/WORKFLOW_DECISION_TREE.md
playbook/PRACTICAL_GUIDE_OUTLINE.md
playbook/DESIGN_RATIONALE.md
```

The next phase should turn the design into a tested procedural toolkit rather than continuing to expand narrative documentation indefinitely.

---

# Priority 1 — Normalize and review the capability map

## 1.1 Audit every matrix row against evidence

For each `CAPABILITY_MATRIX.tsv` row:

- confirm the status label;
- confirm the stated condition boundary;
- confirm the failure mode is actually observed or clearly labeled inference;
- confirm the workaround is validated or status-qualified;
- verify the evidence path still exists;
- add missing experiment/evidence pointers where needed.

## 1.2 Add evidence-strength metadata

Consider adding fields such as:

```text
observation_type
last_verified_date
volatile_fact_recheck
primary_evidence
secondary_evidence
```

Do this only if the added structure improves real session use rather than making the matrix cumbersome.

## 1.3 Generate a human-readable view

Potential outputs:

- `CAPABILITY_MATRIX.md` grouped by domain;
- compact capability cards;
- JSON or YAML export for adapters;
- filtered views for software engineering, scientific Python, HPC, or artifact preservation.

---

# Priority 2 — Implement the first procedural recipes

Implement these first because they are broadly reusable and strongly evidence-backed.

## 2.1 `recipes/runtime-health-check.md`

Status target: `VERIFIED`

Should cover:

- OS/architecture;
- CPU visibility;
- memory/swap;
- GPU/device exposure;
- PID 1 start time and runtime age;
- compiler/toolchain presence;
- Python interpreter paths;
- existing scientific stack;
- network/mirror route checks;
- detection of reset versus continuity.

Validation criterion:

A fresh session should be able to characterize the current runtime without repeating the whole runtime study.

## 2.2 `recipes/classify-connector-failure.md`

Status target: `VERIFIED_WITH_CONDITIONS`

Decision sequence:

```text
resource visibility
→ permission
→ action availability
→ representation compatibility
→ provider-originated failure
→ repository-native automation path
```

Use Drive and GitHub Release experiments as worked examples.

## 2.3 `recipes/verified-archive-packaging.md`

Status target: `VERIFIED`

Should cover:

```text
freeze tree
→ FILE_INVENTORY.tsv
→ MANIFEST.sha256
→ source verification
→ PAX tar stream
→ Zstandard compression
→ archive SHA-256
→ full extraction
→ content hash verification
→ type/mtime_ns/symlink/file-size checks
→ verification sidecar
```

Avoid treating directory inode size as a stable round-trip property.

## 2.4 `recipes/drive-large-archive-backup.md`

Status target: `VERIFIED_WITH_CONDITIONS`

Should cover:

- transferable file reference requirement;
- minimal upload probe;
- current relay-limit recheck;
- deterministic chunking;
- `CHUNKS.sha256`;
- reassembly instructions;
- upload;
- raw-download round trip;
- chunk hash verification;
- reassembly;
- final SHA-256 and integrity check.

---

# Priority 3 — Test the playbook in a genuinely new project session

The next major validation should not be another synthetic capability experiment.

Use a real new project with no dependence on the current conversation history.

Provide only the bootstrap brief and repository location.

Evaluate whether the fresh session:

1. characterizes the project before acting;
2. chooses a sensible topology;
3. distinguishes sandbox, local, CI, Drive, and HPC roles;
4. rechecks volatile facts without redoing solved experiments;
5. reads only relevant playbook material;
6. states capability status and conditions correctly;
7. identifies tradeoffs and known failure modes;
8. chooses appropriate checkpoint and raw-artifact preservation paths;
9. avoids assuming upload success equals exact recovery;
10. avoids treating `NOT_OBSERVED` as impossible;
11. does not force the sandbox as the universal execution environment;
12. reaches productive project work faster than a baseline fresh session.

Record the failures as playbook design feedback, not as hidden prompt adjustments.

---

# Priority 4 — Create project-local workflow output

Design a template such as:

```text
PROJECT_WORKFLOW.md
```

The fresh session should generate this only after project characterization.

Suggested fields:

```text
Project goal
Source of truth
Interactive execution location
Heavy compute location
Dependency strategy
Checkpoint strategy
Raw input/output preservation
Large artifact storage
CI/reproducibility path
Publication path
Known environment-sensitive facts
Unverified assumptions
Fallbacks
```

This file is project-specific and should not be confused with the general playbook.

---

# Priority 5 — Build product-specific adapters

Do this only after the product-neutral playbook and recipes prove useful in fresh-project testing.

## 5.1 Codex adapter

Potential outputs:

```text
AGENTS.md template
Codex skill directories for stable recipes
```

Goals:

- keep standing project guidance concise;
- avoid putting the whole playbook into `AGENTS.md`;
- convert stable procedural recipes into skills where appropriate.

## 5.2 Claude Code adapter

Potential outputs:

```text
CLAUDE.md template
.claude/skills/<recipe>/SKILL.md
```

Goals:

- keep always-loaded project instructions focused;
- move multi-step or narrowly triggered workflows to skills;
- preserve the product-neutral playbook as canonical source.

## 5.3 ChatGPT adapter refinement

Refine:

```text
CHATGPT_SESSION_BOOTSTRAP.md
```

Possible future forms:

- minimal paste-in prompt;
- longer project-start brief;
- evaluation checklist;
- task-specific starter prompts that point to specific recipes.

Do not duplicate the full playbook into every prompt.

---

# Priority 6 — Expand recipes by domain

## Repository workflow

- `existing-repo-start.md`
- `new-project-repository-bootstrap.md`
- `multi-file-refactor-publication.md`

## Dependency provisioning

- `python-overlay-environment.md`
- `native-dependency-bootstrap.md`
- `debian-source-package-build.md`
- `wrong-base-interpreter-debug.md`

## Native development

- `native-project-pilot.md`
- `incremental-rebuild-loop.md`
- `gdb-targeted-debug.md`
- `post-edit-clean-state-check.md`

## Scientific Python / model workflows

- `scientific-python-cpu-pilot.md`
- `checkpoint-provenance.md`
- `same-seed-reproducibility.md`
- `scientific-output-semantic-validation.md`
- `thread-cap-benchmark.md`

## Preservation and recovery

- `compact-mid-experiment-checkpoint.md`
- `freeze-and-inventory-tree.md`
- `runtime-reset-triage.md`
- `restore-chunked-archive.md`
- `semantic-rerun-recovery.md`

---

# Priority 7 — Validate untested architecture paths

These should remain explicitly unverified until tested end-to-end.

## 7.1 GitHub Actions-mediated Release publication

Current status:

```text
PLAUSIBLE_NOT_TESTED
```

Potential experiment:

```text
connector-supported repository mutation
→ push-triggered workflow
→ tag creation
→ draft or normal Release creation
→ asset upload
→ connector-side verification of resulting repository state
```

Questions:

- can the workflow be introduced and triggered entirely through currently available repository operations?
- what token permissions are required?
- can large assets be transferred from session/Drive into the workflow safely?
- how should draft/publication state be separated from backup state?

## 7.2 HPC handoff workflow

Current status:

```text
NOT_OBSERVED / PLAUSIBLE_NOT_TESTED depending on component
```

Potential future study:

- source sync from Git;
- job submission path;
- environment activation;
- input staging;
- checkpoint retrieval;
- result collection;
- provenance link back to repository commit and job metadata;
- raw artifact preservation.

The goal should not be to prove that the sandbox replaces HPC. The goal should be to validate a practical hybrid topology.

---

# Priority 8 — Improve backup and storage hygiene

Current Drive backup state contains historical duplicate backup folders created during experimentation.

Future cleanup tasks:

- choose one canonical folder layout;
- preserve all unique artifacts and hashes before deleting duplicates;
- create a folder-level index or manifest;
- document historical versus canonical assets;
- ensure CPython and Foundry rerun recovery instructions remain self-contained;
- keep original lost Foundry archive checksum and historical verification sidecar distinct from the rerun archive.

Do not perform destructive cleanup without a complete duplicate/content audit.

---

# Priority 9 — Consider machine-readable adapters only after usage data

Possible future outputs:

```text
playbook.yaml
capabilities.json
recipe index with trigger metadata
```

Do not add these merely for abstraction purity.

First observe how fresh sessions actually consume:

- the matrix;
- the decision tree;
- the bootstrap brief;
- the first recipes.

Then add machine-readable forms where they reduce ambiguity or enable reliable adapter generation.

---

# Deferred non-goals

The following are intentionally not immediate tasks:

- turning the entire playbook into one giant system prompt;
- assuming ChatGPT will automatically load repository instructions in every context;
- converting every paragraph into a skill;
- making sandbox execution the default for all workloads;
- claiming universal platform guarantees from one runtime snapshot;
- treating Drive as a universal publication platform;
- treating GitHub Release as a prerequisite for immediate evidence durability;
- deleting historical evidence because a newer rerun exists.

---

# Suggested next-session starting point

When work resumes, start with:

```text
1. read DESIGN_RATIONALE.md
2. audit CAPABILITY_MATRIX.tsv
3. implement runtime-health-check.md
4. implement classify-connector-failure.md
5. implement verified-archive-packaging.md
6. implement drive-large-archive-backup.md
7. test the playbook in a genuinely new real project session
```

The main success criterion for the next phase is not documentation volume.

It is:

> A fresh project session should reach correct, capability-aware project execution faster and with fewer repeated experiments than an uninformed session.
