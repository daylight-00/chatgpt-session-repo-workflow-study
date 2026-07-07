# Workflow Decision Tree

## Purpose

Use this decision tree at the beginning of a real project in a new ChatGPT session.

It does not force a single architecture. It selects among:

```text
session sandbox
local workstation
external cluster / HPC
GitHub-native automation
Google Drive artifact storage
hybrid composition
```

The decision should be revisited when the workload changes. A project can start in the session sandbox for inspection and targeted debugging, then move heavy compute to a cluster while keeping source state in Git and raw evidence in Drive.

## Step 0 — Recheck volatile facts, not established methods

Before planning, recheck only the surfaces that may have changed:

```text
current connector actions
current repository visibility and permission
current runtime CPU / memory / device exposure
current network and mirror routes
current availability of required external services
```

Do not automatically repeat prior capability experiments such as:

```text
Can native C/C++ compile at all?
Can Drive preserve chunked raw archives?
Can the GitHub connector create branches and PRs?
```

unless current-state checks provide evidence that the environment has changed materially.

---

# Decision 1 — What is the project's source of truth?

## Existing Git repository?

```text
YES
  → inspect remote repository through the available GitHub path
  → identify default branch, current commit, active PR/branch state
  → decide whether local/session working tree is needed

NO
  → decide whether this is exploratory scratch work or a durable project
```

### Exploratory scratch work

Use the session workspace temporarily, but define the transition point at which source must move into a repository.

### Durable project

Create or select a repository before substantial divergence accumulates.

## Rule

```text
runtime filesystem
  active mutable workspace

Git repository
  source and narrative state of record
```

Do not treat the session filesystem as the only copy of valuable source work.

---

# Decision 2 — Where should execution happen?

## 2A. Is the work primarily code inspection, transformation, targeted testing, or incremental CPU development?

```text
YES
  → session sandbox is a strong candidate
```

Verified examples:

- shell/Python automation;
- C/C++ compile/link/run;
- targeted tests;
- CPython incremental native build and GDB debugging;
- scientific Python data processing;
- small-batch CPU PyTorch inference for the tested LigandMPNN case.

Then ask:

### Does the workload fit the observed memory envelope?

```text
YES
  → continue in sandbox

UNKNOWN
  → run a small probe and measure RSS

NO
  → use external compute
```

### Is the command long and monolithic?

```text
YES
  → split into resumable phases
  → preserve intermediate state
  → prefer incremental rebuild/test cycles
```

## 2B. Does the work require GPU/CUDA, multi-GPU, high memory, SLURM, or long scheduler-managed jobs?

```text
YES
  → external cluster/HPC is the default candidate
```

The study did not establish these capabilities inside the sandbox.

Recommended hybrid topology:

```text
Git repository
  source + scripts + configs
        ↓
external cluster/HPC
  heavy compute
        ↓
selected outputs + logs + manifests
        ↓
Git for small reproducibility material
Drive or project storage for large raw artifacts
```

The exact cluster handoff path remains project-specific and should be established early.

## 2C. Does the project require interactive GUI tools or desktop visualization?

```text
YES
  → local workstation or remote desktop environment is the default candidate
```

Use the session for:

- scripts;
- data conversion;
- headless validation;
- report generation;
- repository operations;

rather than assuming the sandbox should become a desktop environment.

## 2D. Is the work CI/reproducibility automation rather than interactive development?

```text
YES
  → GitHub Actions or another CI system is a strong candidate
```

Use session execution for exploration and debugging, then encode stable checks into CI when appropriate.

---

# Decision 3 — How should dependencies be provisioned?

Start with the smallest viable closure.

```text
Required dependency
        ↓
Already present?
  YES → use and record version
  NO  ↓
Python package?
  YES → try available internal PyPI / uv path
  NO  ↓
Debian/native package?
  YES → try available internal Debian binary/source mirror
  NO  ↓
Project provides fallback subproject/wrap?
  YES → stage and use fallback
  NO  ↓
Small source-build closure?
  YES → private-prefix source build
  NO  ↓
Large scientific environment?
  → consider prepacked env, internal channel, container, or external execution environment
```

## Avoid the wrong base interpreter trap

Before creating a venv, check:

```text
which python
python -V
python -c 'import torch; print(torch.__version__)'
```

A child venv created from the wrong base interpreter may not inherit the environment that contains the required CPU Torch stack.

## Exact environment parity question

Ask explicitly:

```text
Do we need functional validation?
or
Do we need exact officially supported environment parity?
```

These are different goals.

A compatibility environment can prove functionality while still requiring a caveat about official support range.

---

# Decision 4 — What should be checkpointed before long work?

Before an expensive build, model run, or data transformation, define:

```text
input provenance
source commit / branch / archive hash
config and command line
package/environment versions
checkpoint/model hash
expected output location
minimal success criteria
```

## For valuable long-running work

Use this progression:

```text
inputs/provenance fixed
        ↓
small functional probe
        ↓
compact checkpoint of scripts + logs + outputs so far
        ↓
full run
        ↓
raw archive + manifest + inventory
        ↓
off-runtime backup
```

Do not wait until the final documentation phase to preserve all evidence.

---

# Decision 5 — How should artifacts be preserved?

## Small text/source/config/log files

Preferred destinations:

```text
Git
  when they belong in project history

persistent artifact handoff
  when they are user-facing session deliverables
```

## Large raw input/output/build/environment bundle

Preferred immediate durability path verified in this study:

```text
freeze tree
→ inventory
→ SHA-256 manifest
→ archive
→ verify
→ split if connector relay requires it
→ Drive upload
→ raw-download round trip
→ chunk hash verification
→ reassembly
→ final archive hash and integrity verification
```

## Do not confuse these states

```text
archive exists locally
        != durable backup

upload call succeeded
        != exact recovery verified

historical checksum exists
        != archive bytes are available

Drive backup exists
        != public redistribution has been reviewed
```

---

# Decision 6 — Is a connector failure about access, representation, or capability?

Use this sequence:

```text
1. Can the connector see the resource?
   NO → resource scope / installation access

2. Does the identity have permission?
   NO → authorization problem

3. Does the desired connector action exist?
   NO → capability-surface limitation

4. Does the action accept the representation being passed?
   NO → handoff / argument representation boundary

5. Did the request reach the provider and fail there?
   YES → provider API semantics
```

## Drive example

```text
file exists at /mnt/data
        !=
Drive upload action can receive it
```

Resolve the file into a transferable runtime/connector file reference first.

## GitHub Release example

```text
repo visible + admin permission
        !=
direct Release action exposed
```

Recheck the current connector surface. If still absent, choose among:

- manual publication;
- external credential/tooling environment;
- repository-native automation, after end-to-end validation.

---

# Decision 7 — When should GitHub Actions be used?

Use GitHub Actions when the desired behavior is naturally repository-native:

- reproducibility CI;
- build/test on push or PR;
- scheduled checks;
- release automation, if the workflow and trigger path are validated.

Do not use Actions merely because the session sandbox can also execute commands.

Likewise, do not use the session sandbox merely because Actions could run the same test.

Choose according to lifecycle:

```text
interactive diagnosis / iterative edit-test loop
  → session sandbox often better

repeatable repository policy / CI
  → GitHub Actions often better

accelerator or scheduler workload
  → external compute
```

---

# Decision 8 — What happens if the runtime resets?

Assume the active runtime may disappear.

Recovery order:

```text
1. inspect current PID/runtime age and package state
2. inventory what local state remains
3. compare against Git source of truth
4. check Drive/raw backup availability
5. verify hashes before trusting rehydrated artifacts
6. restore only the environment needed for the next task
7. resume from compact checkpoint or verified archive
```

If exact raw bytes are lost but provenance is complete:

```text
rerun may provide semantic recovery
```

but it must be labeled as a new evidence object, not as exact reconstruction of the lost archive.

---

# Recommended topology patterns

## Pattern A — Lightweight repository project

```text
GitHub repo
  source of truth

session sandbox
  edit + test + debug

GitHub PR
  publication of source changes
```

## Pattern B — Scientific CPU debugging project

```text
GitHub repo
  source + scripts

session sandbox
  small data parse + targeted tests + CPU probe/inference

Drive
  large raw outputs / environment evidence
```

## Pattern C — HPC research project

```text
GitHub repo
  source + configs + reproducibility scripts

session sandbox
  inspection + preparation + lightweight tests + report generation

cluster/HPC
  long/GPU/high-memory compute

project storage / Drive
  selected raw artifacts and recovery bundles
```

## Pattern D — CI-centered software project

```text
session sandbox
  diagnose and iterate

GitHub repo
  source of truth

GitHub Actions
  repeatable build/test policy

Release or artifact storage
  curated distribution, when publication path is validated
```

---

# Final rule

> Select a topology before selecting a workaround. Use the study as a capability map, not as a command to run every project inside the session sandbox.
