# CodeFreeze 2 Lab (Medium)

## Quick Overview

---

### Initial Access

**Q01/** copilot-suggestions-plus-3.2.8.zip

as we begin, we upload the MFT to MFTexplorer, and there will find our first answer

---

### Execution

**Q02/** 2026-04-10 07:52

**Q03/** 2026-03-01 11:50

**Q04/** Copilot Suggestions Plus

**Q05/** onStartupFinished

Beside question 3, the answers are in this file ``C:\Users\Administrator\Desktop\Start Here\Artifacts\disk image\C\Users\nerfjtron\.vscode\extensions\github-team.copilot-suggestions-plus-3.2.8\package.json``. For Q5 its in UNIX Format *1775807550571* that need to be converted. 

Back to 3, i parsed MFT with MFTCmd.exe ``MFTECmd.exe -f "C:\Users\Administrator\Desktop\Start Here\Artifacts\disk image\C\$MFT" --csv .`` and found the info

---

### Command and Control

**Q06/** TelemetryService

**Q07/** telemetry.vscode-plus.mwsdsad.xyz

both are founded in ``C:\Users\Administrator\Desktop\Start Here\Artifacts\disk image\C\Users\nerfjtron\.vscode\extensions\github-team.copilot-suggestions-plus-3.2.8\out\extension.js``

since the 7 question asked for encoded code, i started searching by '==' since generally we founded in the end of BASE64 encode, and i got this *dGVsZW1ldHJ5LnZzY29kZS1wbHVzLm13c2RzYWQueHl6Ojg4OA==*, and then followed it class *_initializeConfig* to the one responsible for establishing the connexion. 

---

### Discovery

**Q08/** 2026-04-10 08:03

here i used prefetch (it show last 8 opened times), the i parsed the prefetch of wsl.exe by PECmd.exe ``PECmd.exe -f "C:\Users\Administrator\Desktop\Start Here\Artifacts\disk image\C\Windows\Prefetch\WSL.EXE-1B26B53B.pf" --csv .``, then opened the file with Timeline Explorer, and followed the time, and got the answer. 

---

### Exfiltration

the wsl, windows subsystem for linux, files are found in ``C:\Users\Administrator\Desktop\Start Here\Artifacts\disk image\C\Users\nerfjtron\AppData\Local\Packages\CanonicalGroupLimited.Ubuntu_79rhkp1fndgsc\LocalState\rootfs``

**Q09/** vscode-git-185fe85142.sock

**Q10/** C:\Users\nerfjtron\AppData\Local\Packages\CanonicalGroupLimited.Ubuntu_79rhkp1fndgsc\LocalState\rootfs\tmp\pyright-2915- 7iD0X6PgsW7I

both answer are found in *.bash_history* of the only user existing

**Q11/** sales_data-large-1.csv

and the last one, we use MFTexplorer

---

### Persistence

**Q12/** 9521

by deconding the *KGJhc2ggPiYgL2Rldi90Y3AvdGVsZW1ldHJ5LnZzY29kZS1wbHVzLm13c2RzYWQueHl6Lzk1MjEgMD4mMSkgJg==* found in *.bash_history* we got the port number.

**Q13/** 2026-04-10 09:18

since the persisting mecanisme is cron, so i searched for the file, and got the answer from last modified time 

**Q14/** sysconf.sh

here i used the parsed MFT in Q3 for easy searching.

**Q15/** \rootfs\etc\cron.d\syscheck

following the previous question, we get the answer from the script, with MFTexplorer this time.

---

### Credential Access

**Q16/** 159965361:Jtron1sVerySuperHandS0mE

disclaimer: this part was helped a lot with an IA. 

we found thz ID and password in ``C:\Users\Administrator\Desktop\Start Here\Artifacts\disk image\C\Users\nerfjtron\AppData\Roaming\RustDesk\config\RustDesk.toml``

so with small search you will find that installed RuskDesk have a flaw: CVE-2026-30785.

and to crack the ID and password we need MachineGuid found in HKLM\SOFTWARE\Microsoft\Cryptography 

so with the crypted ID/Password and MachineGuid, passed throw IA to get the answer.

---

### Lateral Movement

**Q17/** 2026-04-10 12:18

here we will answer this question and last one too since its the same logs, the logs are found here ``C:\Users\Administrator\Desktop\Start Here\Artifacts\disk image\C\Windows\ServiceProfiles\LocalService\AppData\Roaming\RustDesk\log\server\RustDesk_rCURRENT.log``

for Q17 we searched by Connection opened, for the first found.

for Q19 we sarched by Connection closed, for the last found.

one things, in question the answer is requested in UTC, so we need to change it by -7 hours.

---

### Collection

**Q18/** Credential for works

the logs are found in ``C:\Users\Administrator\Desktop\Start Here\Artifacts\disk image\C\Users\nerfjtron\AppData\Roaming\Evernote\logs\evernote.log`` and search by *title* and compare the time between the start and the end of the RustDesk session.

**Q19/** 2026-04-10 12:26

---
