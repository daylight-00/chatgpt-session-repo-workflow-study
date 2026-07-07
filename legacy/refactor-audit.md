# Refactor audit summary

Before the three-part repository restructuring, the project tree and evidence packaging were reviewed.

Main conclusions:

- the project needed structural normalization rather than more experiments;
- the early tar.gz belongs to the Part I historical state;
- compact binary probe evidence required correction;
- Part II retained less raw evidence than the execution pilots;
- CPython had a complete reopenable evidence bundle;
- the first Foundry archive and the later rerun must remain distinct historical objects;
- runtime lifecycle belongs in the evidence model.

The resulting repository separates study documentation, experiment records, compact evidence, large-asset publication metadata, the practical playbook, and legacy project-state records.
