# Session Workflow Playbook — Design Rationale

## Purpose of this document

This document records why the Session Workflow Playbook was designed as a distinct operational layer rather than as a single system prompt, one large instruction file, or a collection of task recipes only.

The study began as a capability investigation:

> Can a ChatGPT session function as a practical, evidence-preserving software project workspace?

The study answered that question positively but conditionally. The next design question was different:

> How should a fresh ChatGPT session use those findings to start and conduct a new real project without repeating the same capability-discovery work?

The answer is the Session Workflow Playbook.

---

# 1. Problem statement

The study accumulated practical knowledge through repeated experiments:

- how the session-local runtime behaves;
- what survives a runtime reset and what does not;
- how artifact handoff differs from local path existence;
- how Google Drive upload depends on a transferable file reference;
- how large archives can be chunked, uploaded, raw-downloaded, reassembled, and hash-verified;
- how GitHub connector repository operations differ from local Git workflows;
- how connector operation availability differs from provider capability and repository permission;
- how dependency acquisition works under restricted public networking but available internal mirrors;
- how native build/test/debug loops can be made resumable;
- how scientific Python and CPU model workflows can be validated in a constrained runtime;
- when CPU oversubscription can make performance dramatically worse;
- how a lost raw evidence archive can be semantically reconstructed from preserved provenance without claiming exact-byte recovery.

A new project session should not have to rediscover these methods from first principles.

At the same time, the study does not justify a universal rule such as:

```text
Always execute the project inside the ChatGPT sandbox.
```

The study established possibilities and boundary conditions. It did not establish a universal optimum.

Therefore the operational layer needs to support **selection**, not just instruction.

---

# 2. Why the name is “Session Workflow Playbook”

The term **onboarding** was considered but rejected as the main name.

Onboarding suggests bringing a new participant into one existing project. The target here is broader: a fresh ChatGPT session may be starting a completely different software or research project. It needs prior operational knowledge about available workflow patterns, tradeoffs, and recovery methods.

The term **instruction file** was also too narrow. Instruction files usually express standing rules for a particular repository or agent environment.

The term **skill set** was closer but still incomplete. Skills are naturally task-specific: package a repeated procedure and invoke it when needed. The playbook must make an earlier decision about which procedure and which execution topology should be chosen at all.

The chosen name reflects the intended scope:

```text
Session
  the operating context is a fresh ChatGPT chat session

Workflow
  the focus is composition of source, execution, dependencies,
  persistence, backup, and publication paths

Playbook
  it presents alternatives, decision logic, known failures,
  and proven procedures rather than one mandatory sequence
```

---

# 3. Relationship to system prompts, AGENTS.md, CLAUDE.md, and skills

The design was informed by existing agent instruction and skill mechanisms, but the playbook is intentionally product-neutral.

## 3.1 System or session-level instruction

A system-level or persistent session instruction is appropriate for broad behavioral contracts:

- evidence discipline;
- safety constraints;
- style and communication rules;
- organizational policies;
- general tool-use expectations.

The Session Workflow Playbook is too large and too conditional to be injected wholesale as a standing instruction.

Many playbook sections are irrelevant to a given project. For example:

- a documentation-only repository does not need scientific CPU inference guidance;
- a cluster-native project does not need the Drive large-archive recipe on every turn;
- a small Python package does not need native CPython build/debug guidance;
- a project with no publication target does not need Release path analysis.

Loading all of this material eagerly would waste context and increase the chance of conflicting or over-generalized behavior.

## 3.2 Codex AGENTS.md

OpenAI Codex documents `AGENTS.md` as a project instruction mechanism. Codex reads `AGENTS.md` files before work, builds an instruction chain from global and project scopes, walks from the project root toward the working directory, and gives more local guidance later in the combined instruction sequence.

This is useful for:

- repository conventions;
- build/test commands;
- project architecture notes;
- standing workflow rules;
- directory-specific overrides.

The playbook serves a prior layer. Before a project-specific `AGENTS.md` can be written, the new session may first need to decide:

- whether the session sandbox should be the main execution environment;
- whether heavy compute belongs on HPC;
- whether GitHub Actions should own repeatable CI;
- whether Drive should hold raw archives;
- which dependency route is appropriate;
- what checkpoint strategy is required.

Therefore the playbook can **inform or generate** project-specific instruction files, but it should not be reduced to one.

## 3.3 Claude Code CLAUDE.md

Claude Code documents `CLAUDE.md` as persistent project/user/org guidance that is read at session start. Its documentation recommends using it for facts that should be held in every session—such as build commands, conventions, project layout, and “always do X” rules—and moving multi-step procedures or narrowly relevant workflows to skills or path-scoped rules.

This distinction strongly supports the playbook architecture:

```text
always-relevant project guidance
  → project instruction file

conditional multi-step procedure
  → recipe or skill

project-topology selection before either exists
  → playbook decision layer
```

It also reinforces the decision not to make one huge always-loaded prompt containing every capability and workaround.

## 3.4 Codex and Claude skills

Codex documents a skill as a directory centered on `SKILL.md`, with optional scripts, references, assets, and other supporting material. Skills may be invoked explicitly or selected when the task matches their description.

Claude Code similarly treats a skill directory as a `SKILL.md` entry point with optional templates, examples, scripts, and reference documentation. Supporting material can remain separate so detailed content is loaded only when needed.

This maps well to the future recipe layer.

Good skill candidates from this study include:

- runtime health check;
- connector failure classification;
- verified archive packaging;
- large Drive archive backup and round-trip verification;
- native incremental build/debug workflow;
- scientific CPU pilot workflow;
- runtime reset triage;
- semantic rerun recovery.

The playbook remains one layer above these skills because it decides **whether a skill should be used and what alternative topology should be considered instead**.

---

# 4. The architectural decision: canonical knowledge plus adapters

The design separates canonical operational knowledge from product-specific representations.

```text
Capability Study
  evidence + experiments + findings
        ↓
Capability Matrix
  compact status-qualified map
        ↓
Workflow Decision Tree
  topology and strategy selection
        ↓
Session Workflow Playbook
  operational knowledge base
        ↓
Recipes
  task-specific procedures
        ↓
Adapters
  ChatGPT bootstrap brief
  AGENTS.md
  CLAUDE.md
  Skills
  PROJECT_WORKFLOW.md
  CI scripts or workflow files
```

The canonical knowledge should remain product-neutral for several reasons.

## 4.1 Agent surfaces change

Connector tools, session capabilities, instruction mechanisms, and product behavior can change. A product-specific representation should not become the only copy of the workflow knowledge.

## 4.2 Different tools need different loading strategies

A ChatGPT chat may need an explicit bootstrap brief.

A coding agent may automatically load repository instructions.

A skill-capable agent may load task procedures on demand.

A CI system may consume scripts and YAML rather than prose.

Keeping one product-neutral knowledge layer allows these forms to be generated or adapted without duplicating the evidence base.

## 4.3 Project-specific topology should be derived, not assumed

The same capability study can support different project plans:

```text
small library
  Git + session sandbox + PR

scientific CPU debugging
  Git + session sandbox + Drive raw archive

HPC protein-design project
  Git + session preparation + cluster compute + external artifact storage

CI-centered project
  session for diagnosis + GitHub Actions for repeatable checks
```

One global instruction file cannot express all of these as simultaneous defaults without becoming contradictory.

---

# 5. Why topology comes before tooling

One of the strongest design principles is:

> Decide where each part of the project should run before selecting detailed tools or workarounds.

The study proved that the sandbox can perform meaningful development work. That success creates a risk of over-generalization.

For example:

- native compilation and GDB debugging were verified;
- scientific CPU parsing and inference were verified under conditions;
- GPU/CUDA execution was not observed;
- high-memory workloads were not established;
- SLURM/HPC execution from the sandbox was not established;
- interactive GUI scientific workflows were not established.

Therefore the correct decision question is:

```text
Where should source live?
Where should interactive iteration happen?
Where should heavy compute run?
Where should raw artifacts be backed up?
Where should repeatable CI run?
How should publication happen?
```

rather than:

```text
How can every part of this project be forced into the session sandbox?
```

---

# 6. Why capability status is explicit

Operational documentation often becomes dangerous when successful experiments are converted into unconditional claims.

The playbook therefore uses a controlled status vocabulary:

```text
VERIFIED
  end-to-end demonstrated

VERIFIED_WITH_CONDITIONS
  demonstrated under specific environment/workload constraints

ENVIRONMENT_SENSITIVE
  usable method, but current surface or environment must be rechecked

PLAUSIBLE_NOT_TESTED
  architecturally reasonable but not validated end-to-end

NOT_OBSERVED
  not demonstrated; this is not proof of impossibility
```

This vocabulary serves several purposes.

## 6.1 Prevent false permanence

A connector action absent in one session may appear later.

A package mirror available in one runtime may differ later.

A current runtime may expose different devices or resources.

The playbook should preserve established methods while still requiring revalidation of volatile facts.

## 6.2 Prevent negative overclaiming

`NOT_OBSERVED` deliberately does not mean `IMPOSSIBLE`.

For example, the study did not observe GPU/CUDA execution in the tested sandbox. That does not prove no future ChatGPT runtime can expose GPU execution.

## 6.3 Preserve evidence boundaries

The original Foundry archive was lost. A rerun reproduced the scientific workflow and produced a new verified evidence bundle. The playbook must not collapse:

```text
semantic reconstruction by rerun
```

into:

```text
exact recovery of the original bytes
```

Status-qualified guidance protects these distinctions.

---

# 7. Why volatile facts are rechecked but solved methods are not rediscovered

The playbook is designed to avoid two opposite failures.

## Failure A — repeating the entire study

A fresh session should not spend hours rediscovering:

- whether native compilation can work at all;
- whether Drive can preserve chunked archives;
- whether a branch/PR workflow is possible;
- how to validate archive round trips;
- how a local path differs from a connector-transferable file reference.

These methods already have evidence.

## Failure B — treating old observations as immutable platform contracts

A fresh session should recheck volatile current facts such as:

- current connector action surface;
- current repository/resource access;
- current runtime CPU/RAM/swap/device exposure;
- current runtime age and continuity;
- current package mirror/network routes;
- current external service availability.

The operational rule is:

```text
reuse established method
+
revalidate volatile prerequisite
```

This is a central rationale for separating capability knowledge from current-state probes.

---

# 8. Why tradeoffs and workarounds are first-class fields

The playbook is not a list of “best practices” detached from context.

Each important capability should answer:

```text
What option exists?
When does it fit?
What did we verify?
What does it cost?
What failure mode is already known?
What workaround exists?
What alternative topology should be considered?
```

Examples from the study:

## Session CPU inference

Useful for targeted scientific debugging and small-batch inference.

Tradeoff:

- limited memory;
- CPU-only in the observed runtime;
- thread oversubscription can be severe.

Known workaround:

- benchmark a conservative thread cap before scaling.

Alternative:

- external GPU/HPC for large or accelerator-dependent workloads.

## Drive backup

Useful for immediate off-runtime durability.

Tradeoff:

- connector file-handoff boundary;
- observed per-object relay ceiling;
- chunk management and duplicate-folder hygiene.

Known workaround:

- transferable runtime asset;
- deterministic chunking;
- chunk manifest;
- raw-download round trip;
- reassembly and final hash verification.

## GitHub Release publication

Useful for curated distribution.

Tradeoff:

- publication and redistribution review are separate from backup durability;
- direct Release action was not observed in the tested connector surface.

Alternatives:

- manual publication;
- external tooling with appropriate credentials;
- repository-native automation after end-to-end validation.

This format is intentionally more useful than a one-line recommendation.

---

# 9. Why preservation and publication are separate

The project lost the original Foundry main archive before an external durable backup existed.

That event changed the operational model.

The playbook now separates:

```text
Immediate preservation
  protect valuable raw evidence now

Curated publication
  review redistribution, organization, naming, and public presentation later
```

The verified emergency pattern is:

```text
meaningful milestone
→ compact checkpoint
→ complete evidence tree
→ manifest + inventory
→ archive
→ verify
→ off-runtime backup
→ raw-download round trip
→ exact recovery verification
```

A public GitHub Release may be an eventual publication target, but it should not delay immediate backup.

---

# 10. Why ChatGPT gets a bootstrap brief instead of automatic instruction discovery

The target operating environment here is an ordinary ChatGPT chat session.

Unlike coding-agent environments that may automatically discover repository instruction files or skill directories, a general chat session should not be assumed to load project files automatically at startup.

Therefore the playbook includes:

```text
CHATGPT_SESSION_BOOTSTRAP.md
```

Its role is deliberately narrow:

- point to the canonical playbook;
- tell the session not to repeat solved capability experiments unnecessarily;
- require project characterization;
- require topology selection;
- require status-qualified claims;
- require source/dependency/checkpoint/artifact/publication paths before substantial work.

It is not intended to duplicate the entire playbook in the chat prompt.

---

# 11. Progressive disclosure as the loading strategy

The desired loading pattern is:

```text
short bootstrap brief
        ↓
capability matrix + decision tree
        ↓
only relevant guide section
        ↓
only relevant recipe
        ↓
project-local instruction or automation artifact
```

This mirrors a broader lesson visible in agent tooling:

- standing project guidance should remain concise and broadly relevant;
- multi-step procedures should be loaded or invoked when relevant;
- supporting references and scripts should remain separate from the minimal entry point.

The playbook adopts that principle without binding itself to a single product.

---

# 12. Alternatives considered

## Alternative A — One large system prompt

Rejected because:

- too much irrelevant material would always occupy context;
- conditional recommendations could conflict;
- product/platform changes would make the prompt stale;
- task-specific procedures would crowd out project reasoning;
- it would encourage treating the study as doctrine.

## Alternative B — One project-level instruction file only

Rejected as the sole representation because:

- the playbook must be useful before a specific project topology is chosen;
- different projects need different derived standing rules;
- ChatGPT chat sessions do not necessarily auto-discover repository instruction files;
- the knowledge should remain reusable across products and projects.

## Alternative C — Skills only

Rejected as the complete architecture because:

- a skill answers how to perform a task, not necessarily whether that task or topology is the right choice;
- selection among sandbox, CI, Drive, HPC, local GUI, or hybrid execution happens before recipe invocation;
- some project decisions concern composition and tradeoffs rather than one procedure.

## Alternative D — Narrative guide only

Rejected because:

- prose alone is hard to query systematically;
- status and caveats become inconsistent;
- failure modes and workarounds are harder to compare;
- machine-readable adaptation becomes difficult.

This motivated the combination:

```text
CAPABILITY_MATRIX.tsv
+
WORKFLOW_DECISION_TREE.md
+
PRACTICAL_GUIDE_OUTLINE.md
+
CHATGPT_SESSION_BOOTSTRAP.md
+
future recipes
```

---

# 13. Current maturity boundary

The playbook foundation is complete enough to preserve the design and support future implementation, but the procedural toolkit is intentionally paused at this stage.

Completed:

- playbook purpose and status model;
- capability matrix;
- topology-first decision tree;
- practical guide architecture;
- ChatGPT session bootstrap brief;
- integration with the root project README.

Not yet completed:

- recipe implementation;
- capability cards or generated human-readable matrix views;
- project-local workflow template;
- AGENTS.md adapter;
- CLAUDE.md adapter;
- Skill adapters;
- blind evaluation in a genuinely new project session;
- GitHub Actions-mediated Release end-to-end experiment;
- HPC handoff end-to-end experiment.

See `TODO.md` for the deferred work plan.

---

# 14. Reference material

The playbook design was informed by, but is not a copy of, the following agent instruction and skill mechanisms.

## OpenAI Codex

### Custom instructions with AGENTS.md

Official documentation:

`https://developers.openai.com/codex/guides/agents-md`

Relevant design observations:

- Codex reads `AGENTS.md` before doing work;
- it composes global and project/directory guidance;
- local guidance can refine broader guidance;
- the mechanism is project-instruction oriented.

### Agent Skills

Official documentation:

`https://developers.openai.com/codex/skills`

Relevant design observations:

- a skill is centered on `SKILL.md`;
- optional scripts, references, and assets can accompany it;
- skills can be invoked explicitly or selected when task matching applies;
- this is a natural model for future stable playbook recipes.

## Anthropic Claude Code

### CLAUDE.md and memory

Official documentation:

`https://code.claude.com/docs/en/memory`

Relevant design observations:

- `CLAUDE.md` provides persistent project/user/org guidance;
- it is read at session start;
- the documentation recommends concise, specific, always-relevant guidance;
- multi-step or narrowly relevant procedures are better moved to skills or scoped rules;
- large always-loaded guidance consumes context and may reduce adherence.

### Skills

Official documentation:

`https://code.claude.com/docs/en/slash-commands`

Relevant design observations:

- Claude Code skills use `SKILL.md` as the entry point;
- templates, examples, scripts, and references can be separated into supporting files;
- supporting files allow detailed material to be loaded only when needed;
- this supports the playbook's progressive-disclosure design.

---

# 15. Design summary

The Session Workflow Playbook is intentionally positioned between a capability study and product-specific execution artifacts.

Its job is to convert evidence into reusable operational judgment:

```text
What can be done?
Under what conditions?
What should be rechecked now?
What topology fits this project?
What tradeoffs matter?
What failure modes are known?
What workaround or alternative exists?
Which recipe should be loaded next?
```

That is why the playbook is not merely onboarding, not merely a prompt, not merely an instruction file, and not merely a skill collection.

It is a **capability-aware workflow selection and execution layer for fresh ChatGPT project sessions**.
