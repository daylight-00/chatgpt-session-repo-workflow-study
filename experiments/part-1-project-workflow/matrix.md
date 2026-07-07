# Experiment matrix

## Session-local to Library promotion

| Probe | Condition | Observed result |
|---|---|---|
| P0 | create file only | not searchable in Library |
| P1 | tool stdout prints `/mnt/data/...` | not searchable |
| P2 | commentary raw `sandbox:` URI | artifact created; searchable next turn |
| P3 | commentary Markdown `sandbox:` link | artifact created; searchable next turn |
| P4 | final raw `sandbox:` URI | artifact created; searchable next turn |
| P5 | final Markdown `sandbox:` link | artifact created; searchable next turn |
| P6 | final basename only | not searchable |
| ABS | assistant absolute `/mnt/data/...` path | not promoted |
| DUP-S | same URI twice in one segment | one logical searchable object observed |
| DUP-X | same path in commentary and final | two logical objects observed |
| DEL | local delete after promotion | Library snapshot retained |
| MOD | local overwrite after promotion | existing Library snapshot unchanged |
| REHANDOFF | overwrite local path, later same-path reference | searchable Library state remained V1 in controlled probe |
| EMPTY | zero-byte regular file | artifact event observed; not found through `file_search` |
| PNG | PNG file | artifact event observed; not found through text search surface |
| ZIP | ZIP file | artifact event observed; not found through text search surface |
| DIR | existing directory URI | no observable artifact object |
| ENOENT | nonexistent path URI | no observable artifact object |
| VIS-0 | same-turn immediate search | absent |
| VIS-20 | same-turn search after ~20 s | absent |
| VIS-NEXT | next-turn search | present |

## GitHub connector probe

| Step | Function | Result |
|---|---|---|
| 1 | `get_repo` | repository metadata and push/admin permissions observed |
| 2 | `fetch_file` | README content and blob SHA returned |
| 3 | `create_branch` | dedicated probe branch created |
| 4 | duplicate `create_branch` | HTTP 422 reference already exists |
| 5 | `create_file` | create commit `6c1bb176...` |
| 6 | `fetch_file` | V1 content; blob SHA `166659ac...` |
| 7 | `update_file` | update commit `e803caba...`; new content SHA `80e3939c...` |
| 8 | `fetch_file` | V2 content confirmed |
| 9 | `delete_file` | delete commit `830842e3...` |
| 10 | `fetch_file` | 404 on deleted path |
| 11 | `update_ref(force=true)` | branch reset to update commit; file visible again |
| 12 | `update_ref(force=false)` | fast-forward to delete commit; file absent again |
| 13 | `compare_commits(main, probe)` | ahead by 3, no final file diff |
| 14 | `fetch_commit` | author and committer associated with `daylight-00` |
