# CodeFreeze 2 Lab (Medium)

## Quick Overview

This CyberDefenders lab is in the Endpoint Forensics category. The scenario has you rebuilding a multi-stage attack from a bunch of different artifact types at once — browser, VSCode, WSL, registry, and app logs — so it's less "one tool does everything" and more "piece it together from wherever the evidence happens to be."

**Tools I used:** MFTECmd, PECmd, Timeline Explorer, Registry Explorer/RECmd, CyberChef (Base64 decoding), DB Browser for SQLite, Python

**MITRE ATT&CK Tactics covered:** Initial Access, Execution, Persistence, Privilege Escalation, Stealth, Discovery, Collection, Command and Control, Exfiltration

> **Disclaimer:** A few parts of this writeup (mainly the Credential Access section) were worked out with help from AI, since it involved cracking encrypted RustDesk credentials that needed more than manual digging. I've flagged it below so it's clear which part that was.

---

### Initial Access
*MITRE ATT&CK Tactics: Initial Access*

**Q01/** copilot-suggestions-plus-3.2.8.zip

To start, we load the `$MFT` into MFT Explorer, and that's where the first answer shows up.

---

### Execution
*MITRE ATT&CK Tactics: Execution*

**Q02/** 2026-04-10 07:52

**Q03/** 2026-03-01 11:50

**Q04/** Copilot Suggestions Plus

**Q05/** onStartupFinished

For Q2, Q4, and Q5, the answers are all in this file:
`C:\Users\Administrator\Desktop\Start Here\Artifacts\disk image\C\Users\nerfjtron\.vscode\extensions\github-team.copilot-suggestions-plus-3.2.8\package.json`

Heads up for Q5 — the timestamp in there is in Unix format (`1775807550571`), so you'll need to convert it.

For Q3, I parsed the MFT with MFTECmd:
```
MFTECmd.exe -f "C:\Users\Administrator\Desktop\Start Here\Artifacts\disk image\C\$MFT" --csv .
```
and found the answer in the output.

---

### Command and Control
*MITRE ATT&CK Tactics: Command and Control*

**Q06/** TelemetryService

**Q07/** telemetry.vscode-plus.mwsdsad.xyz

Both of these are in:
`C:\Users\Administrator\Desktop\Start Here\Artifacts\disk image\C\Users\nerfjtron\.vscode\extensions\github-team.copilot-suggestions-plus-3.2.8\out\extension.js`

Q7 wanted the encoded version, so I searched for `==` in the file since Base64 strings often end with that padding. That got me `dGVsZW1ldHJ5LnZzY29kZS1wbHVzLm13c2RzYWQueHl6Ojg4OA==`. Followed that back to the `_initializeConfig` class, which is the part responsible for setting up the connection.

---

### Discovery
*MITRE ATT&CK Tactics: Discovery*

**Q08/** 2026-04-10 08:03

For this one I used Prefetch, which keeps track of the last 8 times a program ran. Parsed `WSL.EXE`'s prefetch file with PECmd:
```
PECmd.exe -f "C:\Users\Administrator\Desktop\Start Here\Artifacts\disk image\C\Windows\Prefetch\WSL.EXE-1B26B53B.pf" --csv .
```
Then opened the result in Timeline Explorer and worked through the timeline to get the answer.

---

### Exfiltration
*MITRE ATT&CK Tactics: Exfiltration*

The WSL (Windows Subsystem for Linux) files are located in:
`C:\Users\Administrator\Desktop\Start Here\Artifacts\disk image\C\Users\nerfjtron\AppData\Local\Packages\CanonicalGroupLimited.Ubuntu_79rhkp1fndgsc\LocalState\rootfs`

**Q09/** vscode-git-185fe85142.sock

**Q10/** C:\Users\nerfjtron\AppData\Local\Packages\CanonicalGroupLimited.Ubuntu_79rhkp1fndgsc\LocalState\rootfs\tmp\pyright-2915-7iD0X6PgsW7I

Both answers are in the `.bash_history` of the only user on the box.

**Q11/** sales_data-large-1.csv

Back to MFT Explorer for this last one.

---

### Persistence
*MITRE ATT&CK Tactics: Persistence, Privilege Escalation, Stealth*

**Q12/** 9521

Decoding `KGJhc2ggPiYgL2Rldi90Y3AvdGVsZW1ldHJ5LnZzY29kZS1wbHVzLm13c2RzYWQueHl6Lzk1MjEgMD4mMSkgJg==` (also from `.bash_history`) gives you the port number.

**Q13/** 2026-04-10 09:18

Since the persistence mechanism here is a cron job, I searched for the relevant file and got the timestamp from its last-modified date.

**Q14/** sysconf.sh

Reused the parsed MFT from Q3 for this one — much faster than searching from scratch.

**Q15/** \rootfs\etc\cron.d\syscheck

Following on from Q14, the answer is in the script itself, found again through MFT Explorer.

---

### Credential Access
*MITRE ATT&CK Tactics: Credential Access*

**Q16/** 159965361:Jtron1sVerySuperHandS0mE

> **Note:** This part leaned heavily on AI help, as flagged in the disclaimer up top — cracking these encrypted RustDesk credentials needed more than manual digging.

We found the encrypted ID and password in:
`C:\Users\Administrator\Desktop\Start Here\Artifacts\disk image\C\Users\nerfjtron\AppData\Roaming\RustDesk\config\RustDesk.toml`

A bit of research shows that the installed version of RustDesk has a known flaw: **CVE-2026-30785**. To actually crack the ID and password, you also need the `MachineGuid` from `HKLM\SOFTWARE\Microsoft\Cryptography`.

With the encrypted ID/password plus the MachineGuid, I ran it through AI to work out the decryption and get the final answer.

---

### Lateral Movement
*MITRE ATT&CK Tactics: Lateral Movement*

**Q17/** 2026-04-10 12:18

This question and the last one (Q19) both come from the same log file:
`C:\Users\Administrator\Desktop\Start Here\Artifacts\disk image\C\Windows\ServiceProfiles\LocalService\AppData\Roaming\RustDesk\log\server\RustDesk_rCURRENT.log`

For Q17, I searched for "Connection opened" and took the first match. For Q19, I searched for "Connection closed" and took the last match.

One thing to watch out for: the question wants the answer in UTC, so you need to adjust the log's local timestamps by -7 hours.

---

### Collection
*MITRE ATT&CK Tactics: Collection*

**Q18/** Credential for works

**Q19/** 2026-04-10 12:26

Q18's answer is in:
`C:\Users\Administrator\Desktop\Start Here\Artifacts\disk image\C\Users\nerfjtron\AppData\Roaming\Evernote\logs\evernote.log`

Search by "title" and compare the timestamps against the start/end of the RustDesk session to line things up.

---

## Conclusion

This lab was a fun change of pace from the last one — instead of chaining crypto tools together, it was more about hopping between totally different artifact types (VSCode extension files, WSL bash history, prefetch, cron jobs, RustDesk logs, Evernote logs) and keeping the timeline straight in my head.

The RustDesk credential cracking (Q16) was the one part I couldn't fully do on my own — the CVE and the MachineGuid-based decryption were beyond what I could piece together manually, so I leaned on AI for that step, as noted above.

Biggest takeaway: attackers don't stick to one tool or one log source, so neither can you. Half the trail here was sitting in a VS Code extension's own JS file, and the other half was buried in WSL's Linux-side history — an easy thing to miss if you only think in "Windows forensics" mode.

