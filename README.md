# 🛡️ TryHackMe Writeups

![License](https://img.shields.io/github/license/murtrazashirzad-crypto/-tryhackme-writeups-)
![Last Commit](https://img.shields.io/github/last-commit/murtrazashirzad-crypto/-tryhackme-writeups-)
![Repo Size](https://img.shields.io/github/repo-size/murtrazashirzad-crypto/-tryhackme-writeups-)
![Rooms Completed](https://img.shields.io/badge/Rooms-1-blue)
![Path](https://img.shields.io/badge/Path-Cyber%20Security%20101-orange)
![Goal](https://img.shields.io/badge/Goal-OSCP-red)

Personal writeups, notes, and walkthroughs from my journey through **TryHackMe** rooms — working toward **OSCP** and a career in offensive security / penetration testing.

> ⚠️ **Disclaimer:** These writeups are for educational purposes only. All techniques shown were performed in isolated lab environments provided by TryHackMe. Do not use these techniques on systems you do not own or have explicit permission to test.

-----

## 📑 Table of Contents

- [About](#-about)
- [Progress](#-progress)
- [Repository Structure](#-repository-structure)
- [Tools & Methodology](#-tools--methodology)
- [Writeup Template](#-writeup-template)
- [Contributing](#-contributing)
- [License](#-license)
- [Contact](#-contact)

-----

## 📖 About

I’m Taz, a second-year Bachelor of Cyber Security student in Australia focused on **penetration testing** and **offensive security**. This repo tracks my hands-on lab progress as I work toward the **OSCP** certification.

Each writeup follows a consistent format: reconnaissance → enumeration → exploitation → privilege escalation → lessons learned. The goal isn’t just to capture the flag — it’s to **understand why each technique works** so it sticks.

-----

## 📊 Progress

|#|Room                    |Difficulty|Category                        |Status       |Writeup                      |
|-|------------------------|----------|--------------------------------|-------------|-----------------------------|
|1|Blue                    |Easy      |Windows / EternalBlue (MS17-010)|✅ Complete   |[View](./easy/blue/README.md)|
|2|Metasploit: Exploitation|Easy      |Metasploit                      |🚧 In Progress|—                            |

**Legend:** ✅ Complete  •  🚧 In Progress  •  📋 Planned

-----

## 📁 Repository Structure

```
.
├── easy/
│   └── blue/
│       ├── README.md          # The writeup
│       └── screenshots/       # Supporting images
├── medium/
├── hard/
├── insane/
├── templates/
│   └── writeup-template.md    # Use this for every new writeup
├── CONTRIBUTING.md
├── LICENSE
└── README.md
```

Rooms are grouped by **difficulty** first, then named in lowercase-kebab-case (e.g. `easy/blue/`).

-----

## 🧰 Tools & Methodology

**Recon & Enumeration:** Nmap, Gobuster, Nikto, Enum4linux, SMBClient
**Exploitation:** Metasploit, Searchsploit, manual exploits, Burp Suite
**Post-Exploitation:** Meterpreter, Mimikatz, LinPEAS, WinPEAS
**Cracking:** John the Ripper, Hashcat
**Environment:** Kali Linux

I follow the standard pentest kill chain for each room:

1. **Reconnaissance** — passive and active info gathering
1. **Enumeration** — services, versions, users, shares
1. **Exploitation** — gaining initial foothold
1. **Privilege Escalation** — root / SYSTEM
1. **Post-Exploitation** — looting, persistence concepts, lessons
2. | 3 | Gobuster | Easy | Directory Enumeration | ✅ Complete | [View](./easy/gobuster/README.md) |
| 4 | Shells Overview | Easy | Reverse Shells / Web Shells | ✅ Complete | [View](./easy/shells-overview/README.md) |
| 5 | SQLMap | Easy | SQL Injection | ✅ Complete | [View](./easy/sqlmap/README.md) |
| 6 | SOC Fundamentals | Easy | Defensive Security / SOC | ✅ Complete | [View](./easy/soc-fundamentals/README.md) |

-----

## 📝 Writeup Template

Every new writeup is created from [`templates/writeup-template.md`](./templates/writeup-template.md) to keep formatting consistent across the repo.

-----

## 🤝 Contributing

This is primarily a personal learning repo, but corrections, suggestions, and improvements are welcome. See [CONTRIBUTING.md](./CONTRIBUTING.md) for details.

-----

## 📄 License

This project is licensed under the **MIT License** — see the [LICENSE](./LICENSE) file for details.

-----

## 📬 Contact

- **GitHub:** [@murtrazashirzad-crypto](https://github.com/murtrazashirzad-crypto)
- **TryHackMe:** *Add your profile link here*

-----

⭐ If these writeups help you, consider starring the repo!
