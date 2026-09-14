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

**Q15/** 2cd8df5a-25da-4b82-97d0-6dc2ec31b6bd

**Q16/** 10005ec0b29400e813d11c48fbbd7b8a724b5d8a8b4dbfdd7cc375c4ea70069c

**Q17/** 0x985de0b2726e749152a1b329c33cc460d3498e890af9a60ca4f26ff5d7e97477

**Q18/** 2026-05-18 20:41, Mohamed Elfeky

**Q19/** ABA Opinion 477R

**Q20/** You already did

---
