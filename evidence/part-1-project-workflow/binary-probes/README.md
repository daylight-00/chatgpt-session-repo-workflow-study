# Binary probe preservation

The original PNG and ZIP probes were tiny binary files used in the File Library handoff experiment. The currently available early tar.gz snapshot did not contain them, creating an evidence-packaging gap.

To keep Git history text-only while preserving exact reconstructible bytes, this directory stores hexadecimal encodings.

Original hashes:

```text
reverse_exp_binary_20260707.png
sha256 b1ff9c8ea3a780bad09b346c423d2d0e46815926879b18e841d928376a946640

reverse_exp_archive_20260707.zip
sha256 155e3a9d2135e7f62c53b1c249947b890471d7f4033d66500f9e196004a89632
```

Reconstruction:

```python
from pathlib import Path
for name in ["reverse_exp_binary_20260707.png", "reverse_exp_archive_20260707.zip"]:
    Path(name).write_bytes(bytes.fromhex(Path(name + ".hex").read_text().strip()))
```
