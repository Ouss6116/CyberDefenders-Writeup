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

Back to 3

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

here i parsed MFT with MFTCmd.exe ``MFTECmd.exe -f "C:\Users\Administrator\Desktop\Start Here\Artifacts\disk image\C\$MFT" --csv .``
for easy searching.

**Q15/** \rootfs\etc\cron.d\syscheck

following the previous question, we get the answer from the script, with MFTexplorer this time.

---

### Credential Access

**Q16/** 159965361:Jtron1sVerySuperHandS0mE

---

### Lateral Movement

**Q17/** 2026-04-10 12:18

---

### Collection

**Q18/** Credential for works

**Q19/** 2026-04-10 12:26

---
