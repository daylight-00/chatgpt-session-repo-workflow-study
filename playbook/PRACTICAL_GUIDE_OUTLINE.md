# Practical Guide Outline

## Working title

**Session Workflow Playbook: Capability-Aware Project Execution in ChatGPT Sessions**

The title is intentionally broader than onboarding.

The guide is meant to help a fresh ChatGPT session start a real project without repeating the capability-discovery experiments from this study.

Its job is to answer:

```text
What are the viable options?
Which option fits this project?
What has been verified?
What is environment-sensitive?
What tradeoffs matter?
What failure modes are already known?
What workaround or alternate topology should be used?
```

---

# Delivery model

The guide should be maintainable as one knowledge base but usable through several delivery forms.

## Layer A — Session bootstrap brief

A short prompt or attached brief for a new ChatGPT session.

Purpose:

- tell the session that a tested capability map exists;
- prevent unnecessary rediscovery;
- require topology selection before substantial work;
- require status-qualified reasoning;
- point to the playbook location.

Target size: short enough to paste into a new chat.

## Layer B — Capability map

Machine-readable and human-readable capability records.

Primary artifact:

```text
CAPABILITY_MATRIX.tsv
```

Future derived forms may include:

```text
CAPABILITY_MATRIX.md
capability cards by domain
JSON/YAML export for agent tooling
```

Purpose:

- answer whether a path was verified;
- expose conditions and caveats;
- identify known failures and workarounds;
- point back to evidence.

## Layer C — Workflow selection guide

Primary artifact:

```text
WORKFLOW_DECISION_TREE.md
```

Purpose:

- choose source-of-truth topology;
- choose execution location;
- choose dependency path;
- choose checkpoint strategy;
- choose artifact durability path;
- classify connector failures;
- decide whether CI, local compute, sandbox, or HPC is appropriate.

## Layer D — Task recipes

Procedural modules loaded or consulted only when relevant.

These are closest in spirit to agent skills.

Each recipe should contain:

```text
Trigger / when to use
Prerequisites
Verified status
Inputs
Procedure
Validation
Known failures
Fallbacks
Evidence pointer
```

## Layer E — Project-local adaptation

After topology selection, the session may generate project-local operational files such as:

```text
AGENTS.md
CLAUDE.md
project-specific prompt brief
CI workflow
shell scripts
checkpoint scripts
archive/backup scripts
```

The playbook should inform these files; it should not assume any one of them is always the primary interface.

---

# Proposed guide structure

## 00 — How to use this playbook

Explain:

- this is a possibility and workflow-selection guide, not a doctrine;
- status vocabulary;
- when to revalidate volatile facts;
- when not to repeat capability experiments;
- observation versus inference discipline;
- relationship between study docs, playbook, recipes, and project-local instructions.

### Deliverables

- `00-how-to-use-this-playbook.md`
- short session bootstrap prompt
- status legend card

---

## 01 — Choose the project topology

Questions:

```text
What is the source of truth?
What requires interactive iteration?
What requires heavy compute?
What must survive runtime loss?
What should be public versus private?
What must be reproducible versus merely exploratory?
```

Compare:

- session sandbox;
- local workstation;
- GitHub repository and PR workflow;
- GitHub Actions/CI;
- Google Drive artifact backup;
- external cluster/HPC;
- hybrid compositions.

### Output

A project-specific topology map:

```text
source
execution
checkpoint
artifact
publication
```

---

## 02 — Start from an existing or new repository

Topics:

- inspect current remote state;
- branch and PR workflow;
- connector/API model versus local git model;
- when a local/session checkout is useful;
- text mutation through connector;
- tree/commit path for structural changes;
- avoiding noisy per-file commit history;
- feature branch + squash PR publication.

### Recipes

- `existing-repo-start.md`
- `new-project-repository-bootstrap.md`
- `multi-file-refactor-publication.md`

---

## 03 — Characterize the execution environment

Minimal checks:

```text
OS / architecture
CPU visibility
memory / swap
GPU/device exposure
compiler/toolchain
Python interpreter paths
existing scientific stack
network/mirror topology
runtime age / PID 1 start
```

Teach the distinction:

```text
visible hardware
!= usable workload capability
```

and:

```text
many visible CPUs
!= unconstrained threading is optimal
```

### Recipes

- `runtime-health-check.md`
- `cpu-threading-probe.md`
- `memory-risk-probe.md`

---

## 04 — Provision dependencies without rediscovering the network topology

Decision order:

1. already present;
2. internal Python package route;
3. internal Debian binary/source route;
4. project fallback/wrap;
5. private-prefix source build;
6. prepacked environment/internal channel;
7. external execution environment.

Topics:

- uv;
- apt/source packages;
- private-prefix builds;
- Meson fallback;
- Conan with private/local remote;
- prepacked Conda environments;
- when Spack/Nix are worth the bootstrap cost;
- exact parity versus functional compatibility.

### Recipes

- `python-overlay-environment.md`
- `native-dependency-bootstrap.md`
- `debian-source-package-build.md`
- `wrong-base-interpreter-debug.md`

---

## 05 — Native build, test, edit, rebuild, debug

Based on the CPython pilot.

Topics:

- resumable configure/build phases;
- conservative parallelism;
- incremental rebuild;
- source modification evidence;
- restoration and clean-state verification;
- native debugger setup;
- test scope selection;
- preserving build/test/debug logs.

### Recipes

- `native-project-pilot.md`
- `incremental-rebuild-loop.md`
- `gdb-targeted-debug.md`
- `post-edit-clean-state-check.md`

---

## 06 — Scientific Python and CPU model workflows

Based on the Foundry pilot and rerun.

Topics:

- minimum dependency closure;
- real input parsing before full inference;
- model-load probe;
- checkpoint provenance and hashing;
- output semantics validation;
- scientific determinism versus byte-level serialization differences;
- fault injection and repair;
- batch scaling;
- thread oversubscription.

### Recipes

- `scientific-python-cpu-pilot.md`
- `checkpoint-provenance.md`
- `same-seed-reproducibility.md`
- `scientific-output-semantic-validation.md`
- `thread-cap-benchmark.md`

---

## 07 — Decide when not to use the sandbox

This section prevents over-generalization from successful pilots.

Triggers for external execution consideration:

- GPU/CUDA requirement;
- multi-GPU work;
- high-memory model;
- SLURM/scheduler job;
- long unmanaged process;
- very large dataset locality;
- interactive GUI/visualization;
- strict official environment that is expensive to reconstruct.

### Topology patterns

- sandbox preparation + cluster compute;
- local GUI + sandbox scripts;
- Git source + cluster execution + Drive evidence archive;
- CI for repeatable checks after sandbox debugging.

### Recipes

- `handoff-to-hpc.md` — initially a design template, not a verified end-to-end recipe;
- `external-compute-result-ingestion.md` — future validation target.

---

## 08 — Connector boundaries and file handoff

Topics:

- resource visibility;
- permission;
- action availability;
- representation compatibility;
- provider API failure;
- why an existing local path may not be a transferable connector asset;
- minimal probe before large upload.

### Recipes

- `classify-connector-failure.md`
- `drive-upload-small-probe.md`
- `connector-file-reference-handoff.md`

---

## 09 — Preserve valuable work before the runtime disappears

Topics:

- runtime reset evidence;
- compact checkpoint before full packaging;
- input/source/checkpoint provenance;
- raw input/output/log retention;
- manifest and inventory;
- verification sidecar;
- separate checksum persistence.

### Canonical preservation pipeline

```text
experiment reaches meaningful milestone
→ compact checkpoint
→ freeze complete evidence tree
→ FILE_INVENTORY.tsv
→ MANIFEST.sha256
→ source verification
→ archive
→ archive SHA-256
→ full extraction
→ content + metadata verification
→ verification sidecar
→ off-runtime backup
```

### Recipes

- `compact-mid-experiment-checkpoint.md`
- `freeze-and-inventory-tree.md`
- `verified-archive-packaging.md`

---

## 10 — Large raw archive backup to Google Drive

Topics:

- local path versus transferable file reference;
- minimal TXT upload probe;
- observed connector relay ceiling;
- deterministic chunking;
- chunk manifest;
- reassembly instructions;
- raw-download round trip;
- final byte-level validation.

### Recipe

`drive-large-archive-backup.md`

Required validation:

```text
all chunk hashes match
reassembled archive SHA matches
archive integrity test passes
optional byte-for-byte local comparison passes
```

---

## 11 — GitHub Release publication and alternatives

Topics:

- durable backup is not publication;
- redistribution review;
- direct connector Release capability status;
- current connector surface recheck;
- manual publication;
- external credential environment;
- repository-native workflow-mediated Release path.

### Status discipline

The Actions-mediated path must remain:

```text
PLAUSIBLE_NOT_TESTED
```

until validated end-to-end.

### Future recipe targets

- `release-readiness-review.md`
- `manual-draft-release-publication.md`
- `actions-mediated-release-probe.md`

---

## 12 — Recover after runtime reset or artifact loss

Topics:

- detect fresh runtime versus continuity;
- inventory surviving local state;
- restore from Git source of truth;
- restore large raw archive from Drive chunks;
- verify hashes before trust;
- rebuild only necessary environment;
- semantic rerun when exact bytes are gone but provenance is complete;
- preserve distinction between historical lost object and new rerun evidence.

### Recipes

- `runtime-reset-triage.md`
- `restore-chunked-archive.md`
- `semantic-rerun-recovery.md`

---

## 13 — Project closure and handoff

Topics:

- source changes merged;
- tests and outputs summarized;
- raw evidence backed up;
- publication readiness recorded separately;
- project-specific operational instructions generated;
- unresolved capability gaps listed explicitly.

### Output template

```text
source state
execution state
verification state
raw artifact state
backup state
publication state
remaining unknowns
```

---

# Recipe schema

Every recipe should use the same structure.

```text
# Recipe name

Status
Trigger
Do not use when
Prerequisites
Inputs
Procedure
Validation
Known failures
Fallbacks
Tradeoffs
Evidence basis
Volatile facts to recheck
```

Example:

```text
Status
VERIFIED_WITH_CONDITIONS

Trigger
A valuable raw archive must be moved off the session runtime.

Do not use when
The file is small enough for a simpler supported publication/storage path and exact recovery verification is unnecessary.

Volatile facts to recheck
Current Drive connector upload action and current per-object relay behavior.
```

---

# Future session-facing forms

The same playbook knowledge can be adapted into several forms.

## ChatGPT session bootstrap prompt

Short, explicit, topology-first.

It should tell the session:

- use the capability map as prior knowledge;
- recheck only volatile surfaces;
- choose topology before heavy work;
- distinguish verified/conditional/untested paths;
- select recipes only when relevant.

## Project starter brief

Generated after project assessment:

```text
PROJECT_WORKFLOW.md
```

Contains the chosen topology for one specific project.

## Agent instruction files

When useful, generate repository-specific files from the chosen topology and project conventions.

The playbook itself should remain product-neutral; adapters can target the instruction mechanisms of the agent environment being used.

## Skills / procedural modules

Recipes with stable triggers and procedures are candidates for conversion into agent skills or equivalent reusable task modules.

The strongest candidates from the current study are:

1. connector failure classification;
2. runtime health check;
3. dependency route discovery;
4. native incremental build/debug loop;
5. scientific CPU probe/inference workflow;
6. compact checkpoint creation;
7. verified archive packaging;
8. large Drive archive backup and round-trip verification;
9. runtime reset triage;
10. semantic rerun recovery.

---

# Next implementation phase

1. Review and normalize `CAPABILITY_MATRIX.tsv`.
2. Convert the highest-value rows into capability cards.
3. Write the first four recipes:
   - `runtime-health-check.md`;
   - `classify-connector-failure.md`;
   - `verified-archive-packaging.md`;
   - `drive-large-archive-backup.md`.
4. Write a short ChatGPT session bootstrap prompt that references the playbook without forcing the sandbox as the default execution location.
5. Test the playbook in a genuinely new project session and record where the session still wastes effort or over-generalizes.
