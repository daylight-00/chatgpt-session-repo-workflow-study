# Part III assessment

## Capability result

The sandbox is more than a script runner. The experiments directly demonstrated:

```text
native compilation
native linking and execution
dependency provisioning
targeted test execution
incremental rebuilds
native debugging
scientific Python execution
CPU PyTorch inference
biomolecular structure parsing
model checkpoint use
scientific output verification
fault reproduction and recovery
```

## Practicality result

### Practical

- Python package development;
- targeted unit tests;
- small and medium native projects;
- C/C++ extension work;
- incremental build/debug loops;
- CPU-side scientific-software debugging;
- bioinformatics utilities and data transformation;
- realistic small-batch CPU inference for the tested MPNN case.

### Conditional

- clean builds of large native projects;
- broad dependency graphs;
- long integration suites;
- large environment bootstrap.

These are workable but benefit from caches, private prefixes, internal mirrors, incremental state, and checkpoint/resume strategies.

### Not established by these pilots

- CUDA availability;
- GPU model inference;
- multi-GPU execution;
- high-memory model workloads;
- SLURM/HPC execution from the sandbox;
- interactive GUI scientific tools.

## Main operational limitation

The later runtime reset demonstrates that successful computation does not imply durable preservation. The strongest operating model is:

```text
sandbox runtime       active computation and development
GitHub connector      remote source/repository state
artifact layer        durable outputs and evidence
external HPC/GPU      heavy compute when needed
```
