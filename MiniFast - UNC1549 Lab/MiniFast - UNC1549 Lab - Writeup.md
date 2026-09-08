---
# MiniFast - UNC1549 Lab (Medium)
---
## Quick overview

A CyberDefenders lab based on investigating and analyzing a network traffic PCAP, and reconstructing the attack chain through the questions below.

---

### Delivery Analysis

**Q01/** getsqldeveloper.it.com

**Q02/** 63.176.135.137

We can answer Q1 and Q2 from Wireshark's resolved addresses.

![Icon](Images/cb0101.png)

**Q03/** UpdateChecker.dll

**Q04/** E5DFF97693BE9D53E401B2DB84FF6D9C1808300876EE7C2C1FB4FC00AB7FB4FB

Q03 is a bit more complicated, because the traffic is encrypted. *I recommend reading up on TLS encryption and its keys before tackling this one.*

First, we extract all the `strings` from msedge.DMP.

![Icon](Images/cb0102.png)

We search for the TLS keys, copy them into a separate file, and load that file into Wireshark.

![Icon](Images/cb0103.png)

Once the traffic is decrypted, we can extract the DLL file via HTTP export, and finally calculate its hash.

![Icon](Images/cb0104.png)

---

### C2 Analysis

**Q05/** 35.159.37.95

**Q06/** updates.getsqldeveloper.it.com

I found Q06 before Q05, by searching for any .exe file in the HTTP export.

![Icon](Images/cb0105.png)

The IP address for Q05 comes from VirusTotal.

![Icon](Images/cb0106.png)

**Q07/** /rg

**Q08/** 400

**Q09/** e4e9281c9c82ea3ab8850120714c1c26

**Q10/** 30000

**Q11/** 146

The rest of the questions are answered in HTTP stream 47.

![Icon](Images/cb0107.png)

---

### Discovery

**Q12/** EC2AMAZ-D93UQJF

**Q13/** Administrator

Q12 and Q13 are both found in HTTP stream 47.

![Icon](Images/cb0108.png)

**Q14/** 11

In HTTP stream 99, there is a double Base64 encoding. Decode the first round to separate the Base64 parts, then decode them again — you'll get 11 commands.

![Icon](Images/cb0109.png)

---

### Collection and Exfiltration

**Q15/** PUT

**Q16/** db_export.sql

**Q17/** svc_crewsync:Av1@tion-Cr3w#2026

**Q18/** sqldev-prod-01.corp.aero

The method is generally PUT, so filter with `http.request.method == PUT`, follow the resulting stream (108), and you'll find all the answers there.

![Icon](Images/cb0110.png)

![Icon](Images/cb0111.png)

---

### Second Stage

**Q19/** svcupdate.exe

**Q20/** D13377117414E8E18498F5338E8B3474F8EC78729BF4DC880808824B0CEC41A2

We extract the file via HTTP export and calculate its hash.

![Icon](Images/cb0112.png)

**Q21/** ransomware

**Q22/** NoMatter

Searching the hash on VirusTotal answers the last two questions.

![Icon](Images/cb0113.png)

---
