# Operation QUICSILVER Lab (Medium)

## Quick Overview

This CyberDefenders lab is in the Malware Analysis and Threat Intel categories. The scenario is a targeted espionage intrusion: you unpack a disguised container (a VHD file), rebuild the payload hidden inside it, reverse the Go implant it drops, and finally use the operator's leftovers (document metadata, shortcut files) for attribution.

I also used a public report about this campaign to help with the malware analysis part: [Seqrite - Operation QUICSILVER](https://www.seqrite.com/blog/operation-quicsilver-china-nexus-actor-targets-myanmar-diplomats-via-vhd-delivered-go-backdoor/)

**Tools I used:** HxD, Autopsy, LECmd, Ghidra, ExifTool

**MITRE ATT&CK Tactics covered:** Resource Development, Initial Access, Execution, Persistence, Privilege Escalation, Stealth, Defense Impairment, Discovery, Collection, Command and Control, Exfiltration

> **Disclaimer:** Parts of this writeup were done with help from AI (mainly in the Malware Analysis section, for reading the exported Ghidra code, and for a couple of questions where it's mentioned below), plus the public report linked above. I wanted to be upfront about it.

---

### Delivery and Execution
*MITRE ATT&CK Tactics: Resource Development, Initial Access, Execution*

**Q01/** conectix

We start by opening the file in HxD and reading the first string. `conectix` is the signature of a VHD file, which confirms it's really a virtual disk.

![Icon](Images/cb0601.png)

**Q02/** PDF

By mounting the VHD and opening it, we can see the name of the file inside, which is made to look like a PDF.

![Icon](Images/cb0602.png)

**Q03/** ftp.exe

**Q04/** `-s"":_ /_ /j/_ /_ /_ /_ /_ /_`

The "PDF" is actually a `.lnk` shortcut file, so we use `LECmd.exe` to parse it. It gives both answers: the program the shortcut really runs, and the arguments it passes.

![Icon](Images/cb0603.png)

**Q05/** CD147EFE37003399E174951927E5FE727A4481756B116F0204A14A64CC62B059

Now we open Autopsy to export all the files from the VHD.

![Icon](Images/cb0604.png)

Then we follow the argument we found in Q04, which leads us to a file that we can read.

![Icon](Images/cb0605.png)

After that I launched the LNK "PDF" to get the `windowsupdate.exe` malware file and calculated its SHA256.

> **Tip:** do this kind of step inside an isolated VM, never on your main machine.

![Icon](Images/cb0606.png)

---

### Malware Analysis
*MITRE ATT&CK Tactics: Persistence, Discovery, Collection, Command and Control, Exfiltration*

For this part, we open the malware in Ghidra and export it to a C file, and we also keep it open in HxD. We'll use both for the following questions, together with the public report linked at the top and some help from AI.

![Icon](Images/cb0607.png)

**Q06/** agent/main.go

I deduced this one, since the main file of a Go project is almost always called `main`.

![Icon](Images/cb0608.png)

**Q07/** hxxps://regupdate.eamakfu49dc28wa.workers.dev/vere0zme82cad

I got this from the report, and you can verify it in HxD by searching for it in the binary.

![Icon](Images/cb0609.png)

**Q08/** MySecretEncryptionKey2025!@#$%

This one is also from the report.

**Q09/** 0x344600

Here I searched for the CA certificate by looking for `BEGIN` (like in a PEM certificate) and got the address where it's stored.

At the same time, I copied the certificate, because we need it for the next question.

![Icon](Images/cb0610.png)

**Q10/** 2025-12-05

I passed the certificate through AI to get the date from it.

**Q11/** 599

According to the report, the value is between 100 and 600, and the answer is that maximum minus 1.

**Q12/** 0x12A05F200

The heartbeat value is in the exported C code. I converted it to hex to get the answer (it's 5,000,000,000 in decimal).

![Icon](Images/cb0611.png)

**Q13/** %s/download?task_id=%s&file_name=%s&chunk_num=%d

Search for `download` in the strings.

![Icon](Images/cb0612.png)

**Q14/** next_chunk

I searched for the JSON structure (`json:`) and this was the first thing I found.

![Icon](Images/cb0613.png)

**Q15/** gui_id

Same idea as the previous question, but this time I found it in HxD.

![Icon](Images/cb0614.png)

**Q16/** main.(\*Agent).resolvePath

Search for `resolvePath`.

![Icon](Images/cb0615.png)

**Q17/** Shell.Application

Search for `comobject`.

![Icon](Images/cb0616.png)

**Q18/** SystemIn.lnk

This one is from the report.

---

### Threat Intelligence
*MITRE ATT&CK Tactics: Resource Development*

For this section we use the files exported earlier from Autopsy.

![Icon](Images/cb0617.png)

**Q19/** Myanmar

All of the documents point to it.

**Q20/** Sumith, Lynn Lynn

With `exiftool.exe` we check the metadata of all the PDFs and find these names.

**Q21/** KONICA MINOLTA bizhub 266i

Same method as the previous question: the Creator Tool field shows `KM_266i`, and a small search on that gives the whole model name.

**Q22/** Myanmar Institute of Strategic and International Studies

The documents mention MISIS, and a small search gives the full name.

**Q23/** My Disk

Here we used `LECmd.exe` again and found the volume label name. I passed it through AI to get the exact answer.

![Icon](Images/cb0618.png)

**Q24/** 80:45:dd:e5:91:6e

Just a little further down in the same LECmd output as Q23.

**Q25/** dad3fa948af98c55a2029c93999661264c37eb432ef42fdef402326393cfbd0a

Here we just continue from Q10 to get it.

---

## Conclusion

This was a long lab, but a fun one because it covers the whole story: a fake PDF inside a VHD, a shortcut that runs the real payload, a Go backdoor, and then some threat intel at the end. Going from a disguised file to attribution (Myanmar, the documents' metadata, the printer model) in one lab was cool.

The part I found hardest was the malware analysis. Reading the exported Ghidra code and finding strings in HxD took time, and I leaned on the public report and AI to make progress, which is why I added the disclaimer at the top. The `.lnk` and metadata questions were much easier.

Main lesson: small artifacts tell a lot. A shortcut file's arguments, a volume label, or the "Creator Tool" field of a PDF can lead you to the attacker's setup.
