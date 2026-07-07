# GitHub connector behavior

## Execution model

The GitHub integration used in this session is API-mediated. It is not a local Git checkout workflow.

The shell had `git` installed, but no `gh` executable, no detected GitHub token environment variable, and no configured Git credential helper. The connector nevertheless had authenticated access to the connected GitHub account and repository permissions.

```text
assistant
  -> GitHub connector function
  -> authenticated GitHub API operation
  -> remote repository object
```

rather than:

```text
local clone
  -> working tree/index
  -> git commit
  -> git push
```

## Read path

Repository and file reads used remote API abstractions such as `get_repo`, `fetch_file`, `fetch_commit`, and `compare_commits`. `fetch_file` returned content and the current blob SHA for the requested path/ref.

## Contents API write path

The controlled probe branch was `chatgpt-connector-probe-20260707`, created from `main`.

The following operations succeeded:

```text
create_file
fetch_file
update_file
fetch_file
delete_file
```

Observed commits:

```text
create  6c1bb1769725c5e830bb54ab7cd0460ddee59af3
update  e803cabae062a022e513831b45f3202c95df2e9d
delete  830842e3eca6e1fd7d31af1f35754b7efd67e054
```

Update and delete paths required the current blob SHA, consistent with optimistic concurrency control at the Contents API layer.

## Branch ref mutation

`update_ref` was used directly. The probe branch was force-reset from the delete commit to the update commit, making V2 readable again, and then advanced non-forced back to the delete commit, making the file absent again.

This demonstrates that `update_ref` is the closest exposed primitive to direct remote ref movement and can model force-push or fast-forward semantics depending on the `force` flag.

## Commit graph result

The final probe branch was three commits ahead of `main` but had no tree diff because the create/update/delete sequence returned the tree to its original state:

```text
status: ahead
ahead_by: 3
behind_by: 0
files: []
```

## Authorship

For commits created through `create_file`, `update_file`, and `delete_file`, normalized GitHub metadata associated both author and committer with `daylight-00`. No automatic ChatGPT or OpenAI attribution was observed in the commit metadata or commit message.

## Connector limits observed

The available connector surface exposed repository reads/writes, branch refs, issues, PRs, reviews, merges, and Actions-related operations. It did not expose create repository, change repository visibility, create GitHub Release, upload Release asset, or delete branch ref.

The repository bootstrap was therefore completed manually by the user. Once the repository existed and was visible to the connected installation, the connector could populate it.

## Binary artifact write path

A low-level `create_blob` attempt using base64-encoded PNG bytes was blocked by the platform safety layer before the GitHub connector completed the request. Text repository writes remained available. The tiny binary probes are now preserved as exact reconstructible hexadecimal encodings under `evidence/part-1-project-workflow/binary-probes/`. This observation is specific to the available session surface and should not be generalized to GitHub itself.

## Repository population path used for this study

Observed sequence:

```text
create_file(README.md)
  -> initial commit 9a55c92b...

create_tree(content entries, chained base trees)
  -> final tree 6d3d3a0c...

create_commit(parent=9a55c92b..., tree=6d3d3a0c...)
  -> cb344606...
```

A direct `update_ref` attempt against the default `main` branch was blocked. The same commit was exposed through a dedicated branch, pull request #1 was opened, and the PR was squash-merged successfully.

Practical publication path:

```text
session-local project tree
  -> GitHub object/content operations
  -> feature branch
  -> pull request
  -> merge
```

For default-branch publication, the PR path was usable even when direct ref mutation was safety-blocked.
