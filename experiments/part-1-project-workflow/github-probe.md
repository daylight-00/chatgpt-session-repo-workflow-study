# GitHub probe details

Repository used for write testing:

```text
daylight-00/sandbox
```

Test branch:

```text
chatgpt-connector-probe-20260707
```

File path:

```text
connector_probe/probe.txt
```

## Commit sequence

### Create

```text
message: test(connector): create probe file
commit:  6c1bb1769725c5e830bb54ab7cd0460ddee59af3
content: v1: created through GitHub connector contents API
```

### Update

```text
message: test(connector): update probe file
commit:  e803cabae062a022e513831b45f3202c95df2e9d
content: v2: updated through GitHub connector contents API
content SHA: 80e3939cfa41a60ef14e5c11591b89cfebb46439
```

### Delete

```text
message: test(connector): delete probe file
commit:  830842e3eca6e1fd7d31af1f35754b7efd67e054
```

## Ref movement

```text
force=true:
chatgpt-connector-probe-20260707 -> e803caba...
```

The file became readable with V2 content.

Then:

```text
force=false:
chatgpt-connector-probe-20260707 -> 830842e3...
```

The path returned 404 again.

## Final comparison

```text
base: main
head: chatgpt-connector-probe-20260707
status: ahead
ahead_by: 3
behind_by: 0
files: []
```

The branch therefore retained a three-commit experimental history while ending with the same tree state as `main`.
