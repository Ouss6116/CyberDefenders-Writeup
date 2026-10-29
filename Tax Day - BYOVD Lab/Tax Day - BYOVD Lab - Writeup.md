# Tax Day - BYOVD Lab (Medium)

## Quick Overview

un medium difficulte lab based on Endpoint Forensics with a touch of malware analysis 

---

### Initial Access

before we start lets open the MFT file with MFT Explorer, and parse the logs with EvtxECmd.exe and open the result with Timeline explorer, for the following answers.  

**Q01/** form_w9.msi

**Q03/** bringetax.com

both question got from Chrome history

![Icon](Images/cb0501.png)

**Q02/** 2026-04-30 18:10

and here from MFT Explorer

![Icon](Images/cb05013.png)

**Q04/** 2026-04-30 18:14

for this one, since the downloaned file was .msi, i serched for the msi exec in Prefetch and parse it with PECmd.exe to get the answer

![Icon](Images/cb0502.png)

---

since we got the time in anwser Q04, so lets add to filter in Timeline Explorer so we can thing easy.

### Persistence

**Q05/** AteraAgent

following the execution time, found the powershell command that installed this RMM.

![Icon](Images/cb0503.png)

**Q06/** Splashtop® Remote Service

and found the second in ProgramFiles and got the registred name from google

![Icon](Images/cb0504.png)

---

### Discovery

**Q07/** AgentPackageRunCommandInteractive.exe

with the search of ParentCommandLine we got it openning CMD

AteraAgent.exe → AgentPackageRunCommandInteractive.exe → cmd.exe / powershell.exe → whoami, net, nltest...

![Icon](Images/cb0506.png)

**Q08/** HKLM\System\CurrentControlSet\Control\Lsa

searching by registry query 'reg query'

![Icon](Images/cb0505.png)

---

### Credential Access

**Q09/** 2026-04-30 19:32

found a lsass with malware detection log

![Icon](Images/cb0507.png)

---

### Stealth & Defense Impairment

**Q10/** sc.exe stop WinDefend

google search for the command

**Q11/** HWAuidoOs2Ec.sys, kd.exe

found it in roaming folder

![Icon](Images/cb0508.png)

**Q12/** HwAudio

log with new service installed

![Icon](Images/cb0509.png)

**Q13/** 2026-04-30 19:35

log or PF 

**Q14/** MsMpEng.exe

**Q15/** 0x2248DC

both question found with IDA, in the main function

![Icon](Images/cb05010.png)

![Icon](Images/cb05011.png)

---

### Credential Access

**Q16/** 2026-04-30 19:36

for last one we search with lsass.dmp

![Icon](Images/cb05012.png)

---

## Conclusion
