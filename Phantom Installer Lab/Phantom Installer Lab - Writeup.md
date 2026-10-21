# Phantom Installer Lab (Easy)

## Quick Overview

good lab that mix endpoint with malware analysis 

---

### Initial Access

before we start, we open the Tirage.ad1 with FTKimager , i have mount it too for easy browsing.

**Q01/** zooomsuppoertoinstall.com

so we begin by check the history of edge.

![Icon](Images/cb0401.png)

**Q02/** ZoomInstallerFull.exe, 60C41ED1281AD3AE521A33938DB291C85D46E78A

then we export all the dowloaded file to work with it.

![Icon](Images/cb0402.png)

---

### Execution and AppDomain Hijacking

**Q03/** vsn.exe, Microsoft Corporation

a small check in VirusTotal with the SHA1 found in previous question.

**Q04/** ZoomInstallerFull.exe.config

**Q05/** ZoomSetupCore

by checking the exported file we can answer both question.

![Icon](Images/cb0403.png)

**Q06/** T1574.014

and this a small google search. 

---

### Persistence and C2

**Q07/** ZoomHelper.exe

and here we start the malware anaylisis part witth dnSpy

![Icon](Images/cb0404.png)

**Q08/** ZoomUpdateTaskUser-S-1-5-21-403280985-4081385913-4248903659-1002

we found it in task widows folder

![Icon](Images/cb0405.png)

**Q09/** C:\Users\Administrator\AppData\Roaming\ZoomUpdate\Update.exe

still in task file

![Icon](Images/cb0406.png)

**Q10/** ZoomSetupCore.payload.Update.exe

back to dnSpy and search by name Updare.exe

![Icon](Images/cb0407.png)

**Q11/** 2026-07-06 12:32

this part i just checked the Prefetch folwder

ps: you can export it, parsre it with CMDend and open it with timelinerexplorer, and got all the infos. 

![Icon](Images/cb0408.png)

---

### Anti-Forensics and Destruction

**Q12/** 2026-07-06 12:39

byebye.exe is the most interessting for the question

![Icon](Images/cb0409.png)

**Q13/** vssadmin.exe delet shadows /all /quiet

opening the previous file with dnSpy and find the command

![Icon](Images/cb0410.png)

**Q14/** 6

browsing a little down and we find the deleted logs.

![Icon](Images/cb0411.png)

**Q15/** 2026-07-06 12:40

and here the same as Q11

![Icon](Images/cb0412.png)

---

## Conclusion

