# [Room Name]

![Difficulty](https://img.shields.io/badge/Difficulty-Easy-green)
![Status](https://img.shields.io/badge/Status-Complete-success)
![Category](https://img.shields.io/badge/Category-Windows-blue)

> **Room URL:** <https://tryhackme.com/room/[room-slug]>
> **Date Completed:** YYYY-MM-DD
> **Time Taken:** ~X hours

-----

## 📋 Overview

One or two sentences on what this room covers and what skills it tests.

**Skills practised:**

- Skill 1
- Skill 2
- Skill 3

-----

## 🎯 Reconnaissance

### Nmap Scan

```bash
nmap -sC -sV -oN nmap-initial.txt <target-ip>
```

**Findings:**

- Port 22 — SSH
- Port 80 — HTTP
- *(etc.)*

-----

## 🔍 Enumeration

What I found and how I found it. Include commands and key output.

```bash
# Example
gobuster dir -u http://<target-ip> -w /usr/share/wordlists/dirb/common.txt
```

-----

## 💥 Exploitation

Step-by-step explanation of how the foothold was gained. Include **why** each step works, not just the commands.

```bash
# Example exploit command
msfconsole -q -x "use exploit/windows/smb/ms17_010_eternalblue; ..."
```

**Screenshot:** *(optional — place in `./screenshots/`)*

-----

## ⬆️ Privilege Escalation

How root / SYSTEM was obtained.

```bash
# Commands used
```

-----

## 🏁 Flags

|Flag|Location                             |Value          |
|----|-------------------------------------|---------------|
|User|`C:\Users\...\user.txt`              |`THM{redacted}`|
|Root|`C:\Users\Administrator\...\root.txt`|`THM{redacted}`|


> 💡 Consider redacting flag values to encourage others to solve it themselves.

-----

## 🧠 Lessons Learned

- **Key takeaway #1:** what I learned that I didn’t know before
- **Key takeaway #2:** mistakes I made and how to avoid them next time
- **Key takeaway #3:** how this technique might apply in a real engagement

-----

## 🔗 References

- [Original CVE / Advisory](https://example.com)
- [Helpful blog post](https://example.com)
- [Tool documentation](https://example.com)