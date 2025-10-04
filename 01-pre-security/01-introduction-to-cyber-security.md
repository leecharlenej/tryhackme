# Introduction to Cyber Security
1. Offensive Security Intro
2. Defensive Security Intro (Incomplete)
3. Careers in Cyber (Incomplete)
---
## Offensive Security Intro
### Terminology
| Term | Definition |
|------| -----------|
| Offensive Security | Involves breaking into computer systems, exploiting software bugs, and finding loopholes in applications to gain unauthorized access. |
| Goal | Understand hacker tactics + enhance system defences. |
| Gobuster | Free and open-source directory and file enumeration tool, used to find hidden directories and files on web servers. |

### Your First Hack
Start Machine. > Browser loads with http://fakebank.thm.

Open terminal. > `gobuster -u http://fakebank.thm -w wordlist.txt dir`
- Companies have admin portal page for staff to do basic admin controls for BAU.
- Portal page might not have been made private.
- `-u`: State website that is being scanned.
- `-w`: Takes list of words to iterate through to find hidden pages.
- HTTP status code classes:
	- 2xx: Success (server fulfilled the request)
	- 3xx: Redirection
- `Status 200: OK`: Success, resource served as requested.
- `Status 301: Move Permanently`: Redirect, resource has permanently moved to another location.

Go the hidden page and hack the bank.

---
## Defensive Security Intro
### Terminology
| Term | Definition |
|------|------------|
| Defensive Security | Prevent intrusions from occurring + detect intrusions when they occur and respond properly. |
| Related tasks  | - User awareness.<br>- Know all systems and devices and document and manage assets.<br>- Update and patch systems against any known vulnerability/ weakness.<br>- Set up preventive security devices (e.g. firewall to control network traffic in and out of system/ network, IPS to block any network traffic that matches present rules and attack signatures).<br>- Set up logging and monitoring devices to detect malicious activities and intrusions. |
| Security Operations Center (SOC) | Team of cyber security professionals that monitor network and its systems to detect malicious cyber security events. |
| Threat Intelligence | |
| Digital Forensics and Incident Response (DFIR) | |
| Malware Analysis | |

### The Scenario
Check SIEM for suspicious activity - Malicious IP address.

Alert SOC lead.

Block IP with firewall rule.

---
### Careers in Cyber
### Terminology
| Term | Definition |
|------|------------|
| Security Analyst | |
| Security Engineer | |
| Incident Responder | |
| Digital Forensics Examiner | |
| Malware Analyst | |
| Penetration Tester | |
| Red Teamer | |