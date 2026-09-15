---
# Crossed Signals Lab (Medium)
---

## Quick overview

---

### Victim Environment

**Q01/** Brave, Signal

this easy start can be found just by turning arround the users folders

---

### Credential Access

by here i reccommended to open MFT Explorer and open the file MFT fount in the artifact

**Q02/** https: //compliance-protectionoutlook.de/verify

**Q03/** Code of Conduct

so with a sreach in Brave history C:\Users\Administrator\Desktop\Start Here\Artifacts\Triage\C\Users\smitchell\AppData\Local\BraveSoftware\Brave-Browser\UserData\Default\history we can find the answer Q2 , following the nextr one by a good search 

**Q04/** C:\Users\smitchell\Desktop\creds_temp.txt

**Q05/** M@r0mal!x2026$ecure

for both of them we can found them grace a MFT Explorer 

**Q06/** CyberShield Compliance Group is it real?

back to Brave history

---

### Remote Access & Persistence

**Q07/** 63.178.118.143, 2026-05-18 21:16

**Q08/** EC2AMAZ-NG9FK1S, AWS

here can be found in WinEevnt Security by filter 4624 ( Remote interactive logon ) , and AWS was deducated from EC2 

**Q09/** Komari

**Q10/** 63.178.118.143:25774, qKtvvO6FdhU92z9UfOISWv

these info found in the Powershell history , C:\Users\Administrator\Desktop\Start Here\Artifacts\Triage\C\Users\smitchell\AppData\Roaming\Microsoft\Windows\PowerShell\PSReadline\ConsoleHost_history.txt  

**Q11/** T1136.001

as found in previous questions, the user has created a local account to assure the persistence, by a google search we got the Mittre Code

---

### Discovery & Collection

from here we used Registery Explorer, and uploaded both of smitchel and administrator NTUSER.DAT, when you use the Regitrery Explorer, first you open the NTUSER.DAT, and you add ntuser.dat.LOG1 and ntuser.dat.LOG2

**Q12/** Meridian, confidential

this one can be found in NTUSER.DAT\Software\Microsoft\Windows\CurrentVersion\Explorer\WordWheelQuery
 
**Q13/** Meridian_Holdings_Acquisition

**Q14/** 4, Meridian_Financial_Statements_2026.xlsx

for both here NTUSER.DAT\Software\Microsoft\Windows\CurrentVersion\Explorer\RecentDocs

---

### Encrypted Communications Recovery

i recommended at the level to read a little bit about DPAPI Masterkey GUID and Signal Encryotion theory

**Q15/** 2cd8df5a-25da-4b82-97d0-6dc2ec31b6bd

this can be found throw MFT Explorer

**Q16/** 10005ec0b29400e813d11c48fbbd7b8a724b5d8a8b4dbfdd7cc375c4ea70069c

two get the hex key we need to go throw 3 steps, first we need recreat the DPAPI Maskerkey, since we found the data resident we can use to following powershell to recreat it : 

```
$hexString = "02-00-00-00-00-00-00-00-00-00-00-00-32-00-63-00-64-00-38-00-64-00-66-00-35-00-61-00-2D-00-32-00-35-00-64-00-61-00-2D-00-34-00-62-00-38-00-32-00-2D-00-39-00-37-00-64-00-30-00-2D-00-36-00-64-00-63-00-32-00-65-00-63-00-33-00-31-00-62-00-36-00-62-00-64-00-00-00-00-00-00-00-00-00-05-00-00-00-B0-00-00-00-00-00-00-00-90-00-00-00-00-00-00-00-14-00-00-00-00-00-00-00-00-00-00-00-00-00-00-00-02-00-00-00-DF-E2-02-20-56-62-A1-AF-77-FA-54-09-4B-6A-A5-C7-40-1F-00-00-0E-80-00-00-10-66-00-00-94-FD-54-20-3D-A8-C8-3D-B4-09-71-5A-29-D0-31-21-74-D3-BF-AC-1A-91-C8-6B-78-60-D7-7C-1A-69-42-39-F8-72-2F-59-4D-38-F2-59-5B-02-D1-CE-DD-1C-60-93-54-22-28-18-EF-B6-D8-F8-6F-77-B3-EB-CD-DE-F3-9F-23-C9-83-DA-63-C7-A0-AD-6F-47-9C-2F-33-F4-1A-20-54-CB-3A-90-74-BA-42-94-AD-53-93-AD-2F-AA-05-21-04-CC-9E-DD-C7-19-57-A4-20-CA-7B-9A-B7-B8-30-59-BB-FF-2B-69-CF-AF-2E-52-F3-57-0D-A0-3F-AB-80-16-08-A2-4A-80-49-A0-D4-8E-79-C1-0C-A6-44-7A-74-74-02-00-00-00-9B-43-90-5B-9B-26-0B-EB-D3-E5-0D-4C-02-3B-AD-EF-40-1F-00-00-0E-80-00-00-10-66-00-00-90-6B-39-A9-42-55-A8-58-8D-20-FD-77-ED-67-76-BB-3C-E0-4D-69-10-A1-34-8B-D7-E2-AE-D0-2E-24-C9-A6-B6-E4-5E-81-DA-9E-29-72-1B-B4-08-C4-F7-AF-D7-BB-82-D3-33-67-32-E1-62-81-D4-08-0A-0A-BD-4C-4B-35-0C-F8-14-CD-4A-37-5A-21-C7-D8-DD-1C-DC-5C-3A-B4-64-A1-68-2C-F5-D0-1C-30-05-D8-FC-46-E0-31-00-BF-36-64-91-17-54-1D-39-DA-AC-15-F0-98-FC-00-14-68-03-00-00-00-7E-01-B2-21-E7-F0-50-43-B6-90-3E-3F-79-7F-2E-BA"

$bytes = $hexString -split '-' | ForEach-Object { [Convert]::ToByte($_, 16) }
[System.IO.File]::WriteAllBytes("C:\2cd8df5a-25da-4b82-97d0-6dc2ec31b6bd", [byte[]]$bytes)
Write-Host "Wrote $($bytes.Length) bytes"
```

now we need to get the master key from the generated binay by using Mimikatz

`mimikatz #dpapi::masterkey /in:"C:\2cd8df5a-25da-4b82-97d0-6dc2ec31b6bd" /sid:S-1-5-21-3865674213-28386648-2675066931-1008 /password:"M@r0mal!x2026$ecure"`

then we get the key from Signal local state file, and decode it and recreat it as binay

```
$b64 = "RFBBUEkBAAAA0Iyd3wEV0RGMegDAT8KX6wEAAABa39gs2iWCS5fQbcLsMba9EAAAABIAAABDAGgAcgBvAG0AaQB1AG0AAAAQZgAAAAEAACAAAAAv/cgaJ/c5+KsGvIXYrvsFEXi+37W6oabacsIiOJBz1QAAAAAOgAAAAAIAACAAAAChReM3vjU7RdUHB/tjL3xlp9S1xGCYmWffSsq+olU1kTAAAADZoZR6jlU+VJJotjtvEuy3q2omGunumssweBbhxVY0afjM4t0/K0YO9aMjkVu9A+ZAAAAA4Tl9pFCRgskJIvh27ihNPBVti+z8zGZmpw6zZsFT13X7T9DYoMqCzSTRAPUBuWWOvm+bWFvSMzOc96Ulot6INA=="
$rawBytes = [System.Convert]::FromBase64String($b64)
[System.IO.File]::WriteAllBytes("C:\signal_local_state_key.bin", $rawBytes)
Write-Host "Wrote $($rawBytes.Length) bytes total"
```

so the masterkey is decrypted, and we have the signal local state file as binay, lets decrypt it now with mimikatz:

`mimikatz # dpapi::blob /in:"C:\dfir_output\signal_local_state_key_noprefix.bin" /masterkey:d0db37ab736930fea9d3834e41f9d1920cf0cbca5b89fc945dcb9e486ffde3ccec165225d1916185deadcdffb2e6da08be5a77c4b91f386f1dd3d10dcaba7fd6`

and with that we got Signal safeStorage key

**Q17/** 0x985de0b2726e749152a1b329c33cc460d3498e890af9a60ca4f26ff5d7e97477

now we can work on SQLCipher key, by using the key in Signal config.json and the decrypted key in previous question, by using the following AES-GCM decryption script:

```
from cryptography.hazmat.primitives.ciphers.aead import AESGCM

key = bytes.fromhex("10005ec0b29400e813d11c48fbbd7b8a724b5d8a8b4dbfdd7cc375c4ea70069c")

# your original Signal encryptedKey value (WITHOUT stripping v10 this time - strip only "v10", keep the rest)
full_blob = bytes.fromhex("7631301ae35adb7db6aea9340340df401adb802d117b7dec7a2709af81429bc561db69f536775200e128588596cea57e13b214e459f38474fc343f5e0c69c4b5eee17d3dcb28d71f01c019252263b3865703ffed92a3930e37ab7206efedb1")

blob = full_blob[3:]  # strip "v10"
nonce = blob[:12]
ciphertext_and_tag = blob[12:]

aesgcm = AESGCM(key)
plaintext_key = aesgcm.decrypt(nonce, ciphertext_and_tag, None)
print("SQLCipher key:", plaintext_key.hex())
```

and with that we get the final key to access the database


**Q18/** 2026-05-18 20:41, Mohamed Elfeky

**Q19/** ABA Opinion 477R

**Q20/** You already did

for the three last questions, they are founded in the Database messages, and for info, time are using Unix time (1779135823) 

---
