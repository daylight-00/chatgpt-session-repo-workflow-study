# GitHub connector Release capability experiment

## Provenance

This report summarizes a low-level capability assessment performed in a separate session and later incorporated into the project.

Original report:

```text
github_connector_release_capability_assessment_low_level.md
sha256 1c94e3b02c2c5a9e6e0d172c64615571171d982eae84e51441091f29f43fe358
```

The original Markdown report is backed up in the project's canonical Google Drive backup folder.

## Question

Can the currently exposed GitHub connector create a GitHub Release in a private repository?

Test repository:

```text
daylight-00/sandbox
visibility: private
default branch: main
archived: false
```

## Direct observations

1. The connector could discover and inspect the private repository.
2. Repository permission for the authenticated owner identity was reported as `admin`.
3. Release-related connector discovery exposed no direct Release creation action.
4. No explicit generic tag-creation action was observed.
5. Workflow tooling supported observation and rerun operations but exposed no workflow-dispatch action.
6. No existing release workflow or manual-dispatch configuration was found in the test repository.

## Conclusion

The observed blocker was not repository visibility and was not demonstrated to be an authorization failure.

```text
private repository access        available
admin repository permission      confirmed
direct Release action            not exposed
explicit tag creation            not observed
workflow dispatch                not exposed
existing release workflow        not found
```

The correct project-level interpretation is:

```text
repository authorization
    !=
connector operation availability
```

## Indirect path assessment

A repository-native automation path remains plausible:

```text
connector repository mutation
→ repository event
→ GitHub Actions workflow
→ tag / Release / asset operation
```

This was not executed in the external-session assessment. The project should keep it labeled unverified until an end-to-end probe is performed.

## Project relevance

The project currently separates:

```text
Google Drive
  immediate emergency durability

Git repository
  source, documentation, compact evidence, manifests

GitHub Release
  curated publication target
```

The connector capability assessment explains why Drive backup can be automated inside the current workflow while Release publication still needs either a newly exposed direct connector action, a validated repository-native automation path, or a manual/external publication step.

## Operational rule

When evaluating a provider-side mutation:

1. verify resource visibility;
2. verify repository/resource permission;
3. inspect the exact connector action surface;
4. inspect lower-level composable primitives;
5. inspect repository-native automation;
6. distinguish direct connector execution from indirect workflow-mediated execution.
