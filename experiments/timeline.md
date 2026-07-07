# Timeline

All dates are 2026-07-07. Session timezone was Asia/Seoul. GitHub commit timestamps below are recorded in UTC where observed.

## File handoff investigation

The sequence was intentionally iterative:

1. create a test text file under `/mnt/data` and expose it through a `sandbox:` link;
2. confirm that it appeared in File Library;
3. create two files, expose one and keep one unreferenced;
4. verify that only the referenced artifact became searchable at that stage;
5. split trigger tests into creation, stdout, basename, raw URI, and Markdown URI cases;
6. separate trigger-emission turns from observation turns;
7. identify that commentary references also promote artifacts after the turn boundary;
8. compare same-segment duplicate references with cross-segment references;
9. test local deletion and local overwrite after promotion;
10. test zero-byte, PNG, ZIP, directory, and nonexistent-path references;
11. hold the same assistant turn open for approximately 20 seconds and confirm that search visibility still did not appear until a later turn;
12. test same-path modified re-handoff behavior.

Selected observed object-creation timing from the experiment record:

```text
stage-boundary duplicate objects: approximately 04:47:16Z and 04:47:29Z
cross-stage duplicate objects:     approximately 04:59:42Z and 04:59:58Z
```

These timings are retained as observations, not as a claim about undocumented backend scheduling.

## GitHub connector investigation

Observed commit timestamps:

```text
05:15:21Z  create probe file
05:15:32Z  update probe file
05:17:18Z  delete probe file
```

After commit creation, the branch ref was moved back to the update commit with a forced ref update, then advanced to the delete commit with a non-forced ref update.
