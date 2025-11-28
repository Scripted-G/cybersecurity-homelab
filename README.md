# Homelab

Hands-on cybersecurity practice environment for vulnerability assessment, penetration testing, and security operations. This repository documents my journey in building practical offensive and defensive security skills through controlled lab exercises.

## 🎯 Purpose

This homelab serves as a safe, isolated environment where I can:
- Practice penetration testing methodologies
- Exploit known vulnerabilities in a controlled setting
- Document security assessments and findings
- Develop security automation scripts
- Build hands-on experience for SOC Analyst and penetration testing roles

## 🛠️ Lab Environment

**Virtualization Platform:** KVM/QEMU on Fedora 43

**Virtual Machines:**
- **Kali Linux** - Primary attacking machine with penetration testing tools
- **Metasploitable 2** - Intentionally vulnerable target for exploitation practice
- **Network Configuration** - Isolated virtual network for safe testing

**Key Tools Used:**
- Metasploit Framework
- Nmap
- Wireshark
- Netcat
- Burp Suite
- Various Kali Linux tools

## 📁 Repository Structure

```
cybersecurity-homelab/
├── setup/              # Homelab configuration and setup guides
├── writeups/           # Detailed penetration testing writeups
├── scripts/            # Security automation scripts (coming soon)
└── README.md           # This file
```

## 🔍 Completed Exercises

### Exploitation & Vulnerability Assessment

1. **vsftpd 2.3.4 Backdoor Exploitation**
   - Successfully exploited known backdoor vulnerability in vsftpd
   - Used Metasploit Framework for exploitation
   - Gained remote shell access on target system
   - [Full writeup →](writeups/vsftpd-exploit-writeup.md)

2. **Home Network Security Assessment**
   - Comprehensive scan of home network infrastructure
   - Identified active hosts and open services
   - Documented security findings and recommendations
   - [Full writeup →](writeups/home-network-assessment-sanitized.md)

3. **Roku API Reconnaissance**
   - Explored Roku device API endpoints
   - Documented available commands and responses
   - Security analysis of exposed interfaces
   - [Full writeup →](writeups/roku-api-reconnaissance.md)

## 📚 Setup Documentation

Want to build your own cybersecurity homelab? Check out my setup guide:

- [Homelab Setup Guide →](setup/homelab-setup.md) - Complete walkthrough of lab configuration

## 🎓 Learning Path

**Current Focus:**
- Metasploit Framework exploitation techniques
- Network reconnaissance and enumeration
- Vulnerability assessment methodologies
- Security documentation best practices

**In Progress:**
- CompTIA Security+ certification (exam scheduled for early 2026)
- TryHackMe learning paths (Pre-Security, Cyber Defense, SOC Level 1)

**Future Goals:**
- Advanced penetration testing techniques
- Web application security testing
- Network security monitoring
- Security automation with Python

## 🔐 Skills Demonstrated

Through this homelab, I've developed practical experience in:

- **Reconnaissance & Enumeration** - Network mapping, service identification, information gathering
- **Vulnerability Assessment** - Identifying and analyzing security weaknesses
- **Exploitation** - Leveraging known vulnerabilities using industry-standard tools
- **Documentation** - Creating clear, professional security writeups
- **Lab Management** - Building and maintaining isolated testing environments

## ⚠️ Ethical Use Disclaimer

All exercises in this repository are conducted in a controlled, isolated lab environment on systems I own or have explicit permission to test. This work is for **educational purposes only** to develop cybersecurity skills for defensive security roles.

**I do not engage in or condone:**
- Unauthorized access to computer systems
- Malicious hacking or illegal activities
- Testing on systems without explicit permission

## 📝 How to Use This Repository

**For Recruiters/Employers:**
- Browse the [writeups/](writeups/) folder to see detailed penetration testing documentation
- Review [setup/](setup/) to understand my lab configuration process
- Check commit history to see ongoing learning and development

**For Fellow Learners:**
- Feel free to use my setup guide as a reference for your own homelab
- Writeups include step-by-step methodology you can follow
- Reach out if you have questions or want to collaborate!

## 🔄 Maintenance & Updates

This repository is actively maintained and regularly updated with:
- New exploitation exercises and writeups
- Additional tools and techniques
- Security automation scripts
- Lab environment enhancements

**Last Updated:** November 2025

## 📫 Connect

- **LinkedIn:** [linkedin.com/in/gabriel-orta-cyber](https://linkedin.com/in/gabriel-orta)
- **GitHub:** [github.com/Scripted-G](https://github.com/Scripted-G)

---

*Building practical cybersecurity skills one exploit at a time.* 🛡️
