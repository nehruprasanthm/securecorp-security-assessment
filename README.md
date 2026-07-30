# SecureCorp Security Assessment

A full-lifecycle security assessment built and executed independently — from lab deployment through attack simulation, SIEM-based detection analysis, and client-ready reporting.

## What this project demonstrates

- **Offensive security**: SQL Injection, Cross-Site Scripting (XSS), Command Injection, and brute-force attacks executed against a deliberately vulnerable web application (DVWA)
- **Defensive security / SOC analysis**: Investigated each attack in a self-deployed Wazuh SIEM to determine whether it was actually detected — and found that 2 of 4 attack types were **not** caught by default detection rules, including the most severe one (Command Injection)
- **Real-world troubleshooting**: Diagnosed and resolved a MySQL 8.4 compatibility bug, an OS/SIEM incompatibility requiring an environment rebuild, a package conflict between Wazuh components, an API authentication mismatch, and a live upstream bug in Hydra (v9.6) — worked around with a custom script
- **Professional reporting**: Full written assessment report (asset inventory, risk assessment, attack findings, incident report, prioritized recommendations) and a client-facing debrief presentation

## Key finding

**Detection coverage does not correlate with attack severity.** The two most severe vulnerabilities found (SQL Injection and Command Injection) produced zero alerts under Wazuh's default configuration, while a medium-severity attack (XSS) was caught automatically — because its payload happened to match a generic pattern-matching rule. This is a genuine, real-world insight into why SIEM deployments require active tuning against an organization's actual application risk, not just installation.

## Environment

| Component | Details |
|---|---|
| Target | Ubuntu 22.04 LTS Server |
| Vulnerable application | DVWA (Damn Vulnerable Web Application) |
| SIEM | Wazuh 4.9.2 (manager, indexer, dashboard) |
| Attacker platform | WSL 2 (Ubuntu) |
| Tools used | Nmap, Hydra, curl, SSH |

## Repository contents

- `SecureCorp_Security_Assessment_Report.docx` — full written report
- `SecureCorp_Client_Debrief.pptx` — presentation deck
- `week1-*.png` — lab deployment evidence (VM, networking, DVWA, Wazuh setup)
- `week2-*.png` — attack execution and SIEM detection evidence

## Attack summary

| Attack | Result | Detected by SIEM? |
|---|---|---|
| Brute Force (SSH + web login) | Passwords cracked in seconds | ✅ Yes (SSH only) |
| SQL Injection | Full user database dumped | ❌ No |
| Cross-Site Scripting | Arbitrary script executed in browser | ✅ Yes |
| Command Injection | Arbitrary OS commands executed | ❌ No |

## Note on scope

This was a personal, self-directed lab project conducted entirely within an isolated environment built for this purpose. No external systems, networks, or third-party assets were accessed at any point.
