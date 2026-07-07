# ChatGPT Session Bootstrap Brief

## Purpose

This is the session-facing entry point for starting a real project in a fresh ChatGPT chat.

Unlike repository instruction mechanisms that an agent runtime may discover automatically, this brief is intended to be explicitly provided or referenced in the chat session.

It does not tell the session to run everything in the sandbox. It tells the session to use the study's capability map as prior operational knowledge and choose an appropriate topology for the new project.

---

## Minimal bootstrap prompt

```text
I want to start a real project in this ChatGPT session.

Use the Session Workflow Playbook in
`daylight-00/chatgpt-session-repo-workflow-study/playbook/`
as prior operational knowledge.

Do not repeat capability-discovery experiments that the study has already completed unless a volatile current fact must be revalidated.

Before substantial work:
1. characterize the project workload;
2. recheck only relevant volatile surfaces such as current connector actions, runtime resources, repository access, and dependency routes;
3. choose an execution topology among the session sandbox, GitHub-native automation, Google Drive artifact storage, local execution, external cluster/HPC, or a hybrid;
4. define source-of-truth, dependency, checkpoint, raw-artifact, and publication paths;
5. distinguish VERIFIED, VERIFIED_WITH_CONDITIONS, ENVIRONMENT_SENSITIVE, PLAUSIBLE_NOT_TESTED, and NOT_OBSERVED capabilities;
6. state tradeoffs, known failure modes, and available workarounds for important choices.

Use only the playbook sections and recipes relevant to this project. Do not assume the sandbox is always the preferred execution location.
```

---

## Expanded bootstrap prompt

```text
I want to start and conduct a real project in this ChatGPT session.

Prior operational knowledge exists in:
`daylight-00/chatgpt-session-repo-workflow-study/playbook/`

Treat that material as a capability map and workflow playbook, not as a rigid prescription.

Startup procedure:

A. Understand the project
- identify whether this is a new or existing repository;
- identify expected languages, native builds, scientific Python, model inference, GUI needs, GPU/high-memory needs, HPC/scheduler needs, CI needs, and artifact sizes;
- identify which outputs are exploratory, reproducibility-critical, or publication candidates.

B. Revalidate only volatile facts
- current connector action surface;
- current repository/resource access and permissions relevant to the task;
- current runtime CPU, RAM, swap, GPU/device exposure, and runtime age;
- current network/mirror routes needed for dependencies;
- current availability of external services required by the selected topology.

Do not repeat completed capability studies merely to rediscover known methods.

C. Choose the topology
For each project component, choose the most appropriate location among:
- ChatGPT session sandbox;
- GitHub repository and pull-request workflow;
- GitHub-native CI/automation;
- Google Drive durable raw-artifact backup;
- local workstation;
- external cluster/HPC/GPU infrastructure;
- a hybrid combination.

D. Establish project state paths before expensive work
Define:
- source of truth;
- active execution location;
- dependency strategy;
- checkpoint strategy;
- raw input/output/log preservation strategy;
- large artifact backup strategy;
- publication strategy.

E. Use status-qualified claims
Label relevant paths as:
- VERIFIED;
- VERIFIED_WITH_CONDITIONS;
- ENVIRONMENT_SENSITIVE;
- PLAUSIBLE_NOT_TESTED;
- NOT_OBSERVED.

For important choices, report:
- why this option fits;
- alternatives;
- tradeoffs;
- known failure modes;
- workaround/fallback;
- what current fact, if any, still needs revalidation.

F. Preserve valuable work incrementally
Do not wait for final documentation before preserving all evidence. For long or valuable work, create a compact checkpoint after meaningful milestones, then produce and verify a complete raw archive when appropriate.

G. Maintain the study's evidence discipline
Separate direct observations from inference. Do not claim a provider limitation when only a connector limitation was observed. Do not claim exact recovery when a semantic rerun created a new evidence object.

After this assessment, begin the project using only the playbook sections and recipes relevant to the chosen topology.
```

---

## Expected first response behavior

A well-oriented new session should not immediately start installing packages or cloning/building blindly.

It should first produce a compact project topology proposal such as:

```text
Source of truth
  GitHub repository X

Interactive development
  session sandbox for targeted edit/test/debug

Heavy compute
  external SLURM cluster

Dependencies
  existing project environment on cluster;
  lightweight local/session overlay only for inspection and unit tests

Checkpoints
  commit/branch checkpoints for source;
  compact raw evidence checkpoint after meaningful scientific milestones

Large raw outputs
  project storage or Drive backup depending on sensitivity and size

Publication
  GitHub PR for source;
  Release only after redistribution review and supported publication path
```

The exact topology should follow the project workload rather than copying this example.

---

## What the new session should not do

```text
Do not assume runtime continuity.
Do not assume a local path is automatically connector-transferable.
Do not infer provider permission failure from a missing connector action.
Do not force GPU/HPC/GUI workloads into the sandbox without evidence.
Do not treat upload success as exact recovery verification.
Do not treat a semantic rerun as byte-identical recovery.
Do not repeat the full capability study unless new evidence requires it.
Do not load every guide section when only one recipe is relevant.
```

---

## Future adaptation targets

This brief may later be translated into:

- a shorter user paste-in prompt;
- a project-specific `PROJECT_WORKFLOW.md` generated after topology selection;
- repository-level agent instruction files;
- skill modules for stable repeatable recipes;
- an evaluation checklist for testing whether a new session uses the playbook correctly.

The canonical knowledge should remain in the product-neutral playbook so that one agent-specific adapter does not become the only representation of the workflow knowledge.
