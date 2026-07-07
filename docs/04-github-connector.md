# GitHub connector behavior

## Execution model

The GitHub integration used in this session is API-mediated. It is not a local Git checkout workflow.

The shell had:

- `git` installed;
- no `gh` executable;
- no detected GitHub token environment variable;
- no configured Git credential helper.

The connector nevertheless had authenticated access to the connected GitHub account and repository permissions.

The effective model is:

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

Repository and file reads used remote API abstractions such as:

```text
get_repo(repository_full_name)
fetch_file(repository_full_name, path, ref)
fetch_commit(repo_full_name, commit_sha)
compare_commits(repo_full_name, base, head)
```

`fetch_file` returned content and the current blob SHA for the requested path/ref.

## Write path: Contents API wrapper

The controlled probe branch was:

```text
chatgpt-connector-probe-20260707
```

It was created from `main`.

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

The update path required the current blob SHA:

```text
fetch_file -> blob SHA 166659ac...
update_file(expected sha=166659ac...)
-> new commit e803caba...
-> new content SHA 80e3939c...
```

Delete likewise required the current blob SHA.

The behavior is consistent with optimistic concurrency control at the Contents API layer: a write identifies the current content object it intends to replace or delete.

## Branch ref mutation

`update_ref` was used directly.

First, the probe branch was force-reset from the delete commit to the update commit:

```text
branch -> e803caba...   force=true
```

The previously deleted file became readable again with V2 content.

Then the branch was advanced to the delete commit using a non-forced update:

```text
branch -> 830842e3...   force=false
```

The file again became absent.

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

For commits created through `create_file`, `update_file`, and `delete_file`, the normalized GitHub metadata associated both author and committer with:

```text
daylight-00
```

No automatic ChatGPT or OpenAI attribution was observed in the commit metadata or commit message.

The connector function schemas did not expose explicit author or committer parameters for these write paths.

This means account-level commit attribution followed the authenticated connected GitHub identity in the tested path.

## Connector limits observed in this session

The available connector surface exposed repository reads/writes, branch refs, issues, PRs, reviews, merges, and Actions-related operations.

It did **not** expose, in the available function surface:

- create repository;
- change repository visibility;
- create GitHub Release;
- upload Release asset;
- delete branch ref.

Because shell-side GitHub credentials were also absent, those operations could not be completed through a CLI fallback in this session.

For this study, the repository bootstrap was completed manually by the user. Once the empty public repository existed and was visible to the connected GitHub installation, the connector could discover it and is suitable for populating repository contents. The limitation is therefore a repository-lifecycle bootstrap boundary, not an inability to work with an already-created repository. Release creation and Release asset upload remain outside the connector surface observed in this session.

## Binary artifact write path

A low-level `create_blob` attempt using base64-encoded PNG bytes was blocked by the platform safety layer before the GitHub connector completed the request. Text repository writes remained available. Binary probe originals are therefore preserved in the timestamp-preserving tar archive rather than committed through this connector path. This observation is specific to the available session surface and should not be generalized to GitHub itself.
