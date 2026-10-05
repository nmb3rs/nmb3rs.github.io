---
title: "Example Writeup: Blind SSRF to RCE"
date: 2026-09-12
description: "A blind SSRF in an image fetcher, escalated to code execution through an internal Redis instance."
tags: ["web", "ssrf", "htb"]
draft: true

# Optional writeup fields — delete the ones that don't apply.
# The box at the top of the post only appears if at least one is set.
platform: "Hack The Box"
machine: "Example"
category: "Web"
difficulty: "Medium"
os: "Linux"
solved: 2026-09-12
---

A one-paragraph summary of the challenge and the bug, so a reader can tell
within ten seconds whether this is the post they were looking for.

## Recon

Start with what you saw. Keep commands in fenced blocks with a language tag so
they get highlighted:

```bash
nmap -sC -sV -oA scan 10.10.11.42
ffuf -u http://target/FUZZ -w /usr/share/wordlists/dirb/common.txt
```

## The bug

Explain the vulnerable behaviour before the payload. The payload makes sense
only once the reader knows what the application was trying to do.

```python
import requests

payload = {"url": "http://127.0.0.1:6379/"}
r = requests.post("http://target/fetch", json=payload, timeout=5)
print(r.status_code, len(r.content))
```

> Quote blocks are handy for the one line of documentation or source that
> explains why the bug exists.

## Exploitation

1. Confirm the request reaches internal hosts
2. Find a service that speaks a line-based protocol
3. Smuggle commands through the URL parser

| Step | Request | Result |
| --- | --- | --- |
| 1 | `http://127.0.0.1:80` | 200 |
| 2 | `http://127.0.0.1:6379` | hang |
| 3 | `gopher://127.0.0.1:6379/_...` | code execution |

## Takeaways

What you'd do differently, and what the fix should have been.

<details>
<summary>Full exploit script</summary>

```python
#!/usr/bin/env python3
# The long version, hidden by default so it doesn't break the flow.
```

</details>
