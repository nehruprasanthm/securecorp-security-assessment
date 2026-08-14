# SecureCorp Security Assessment

A full-lifecycle security assessment built and executed independently — from lab deployment through attack simulation, SIEM-based detection analysis, and client-ready reporting.

## What this project demonstrates

- **Offensive security**: 10 attack executions across 8 techniques — Brute Force (SSH + web login), SQL Injection (in-band + blind, automated with sqlmap), Cross-Site Scripting (reflected + stored), Command Injection, CSRF, and Clickjacking — against a deliberately vulnerable web application (DVWA)
- **Chained exploitation**: combined CSRF with Clickjacking into a full admin account takeover triggered the instant a victim opened a page — no click or form submission required
- **Defensive security / SOC analysis**: investigated every attack in a self-deployed Wazuh SIEM to determine whether it was actually detected — **5 of 8 techniques evaded default detection entirely**, including the most severe finding (Command Injection)
- **Credential exposure**: used sqlmap's built-in cracker to recover all extracted password hashes in under 3 seconds, exposing weak unsalted MD5 hashing
- **Real-world troubleshooting**: diagnosed and resolved 9+ infrastructure and tooling failures — a MySQL 8.4 compatibility bug, an OS/SIEM incompatibility requiring an environment rebuild, a Wazuh package conflict, an API authentication mismatch, and a live upstream bug in Hydra (v9.6) — worked around with custom scripting
- **Professional reporting**: full written assessment report (asset inventory, risk assessment, attack findings, incident report, prioritized recommendations) and a 16-slide client-facing debrief presentation

## Key finding

**Detection coverage tracks payload syntax, not attack severity.** Attacks that produce abnormal, high-volume, or overtly malicious-looking traffic (repeated failed logins, `<script>` tags, automated SQLi tooling) were reliably caught by Wazuh's default rules. Attacks that rely on a single, well-formed request and a broken trust assumption — Stored XSS, CSRF, and Clickjacking — were **completely invisible**, regardless of real-world impact. The two most severe vulnerabilities found (SQL Injection and Command Injection) produced zero alerts, while a medium-severity XSS attack was caught automatically simply because its payload matched a generic pattern.

## Environment

| Component | Details |
|---|---|
| Target | Ubuntu 22.04 LTS Server |
| Vulnerable application | DVWA (Damn Vulnerable Web Application) |
| SIEM | Wazuh 4.9.2 (manager, indexer, dashboard) |
| Attacker platform | WSL 2 (Ubuntu) |
| Tools used | Nmap, Hydra, sqlmap, curl, SSH |

## Repository contents

- `SecureCorp_Security_Assessment_Report.docx` — full written report
- `SecureCorp_Client_Debrief.pptx` — 16-slide presentation deck
- `week1-*.png` — lab deployment evidence (VM, networking, DVWA, Wazuh setup)
- `week2-*.png` — core attack execution and SIEM detection evidence
- `SQLi_Blind_*`, `XSS_Stored_*`, `CSRF_*`, `Clickjacking_*` — extended attack phase evidence

## Attack summary

| Attack | Result | Detected by SIEM? |
|---|---|---|
| Brute Force (SSH + web login) | Passwords cracked in seconds | ✅ Yes (SSH) |
| SQL Injection (in-band) | Full user database dumped | ❌ No |
| SQL Injection (Blind, sqlmap-automated) | Full table dumped, all hashes cracked in <3 sec | ✅ Yes — 5,494 alerts |
| Cross-Site Scripting (Reflected) | Arbitrary script executed in browser | ✅ Yes |
| Cross-Site Scripting (Stored) | Script persists for every visitor | ❌ No |
| Command Injection | Arbitrary OS commands executed | ❌ No |
| CSRF | Admin password silently changed | ❌ No |
| Clickjacking (chained with CSRF) | Full takeover from a single page visit, no click required | ❌ No |

## Note on scope

This was a personal, self-directed lab project conducted entirely within an isolated environment built for this purpose. No external systems, networks, or third-party assets were accessed at any point.
