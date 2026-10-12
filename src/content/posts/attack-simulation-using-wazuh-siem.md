---
title: Attack simulation and detection with Wazuh SIEM
excerpt: A controlled vulnerability assessment lab that connects Kali Linux, vulnerable targets, and Wazuh monitoring across the full attack workflow.
pubDate: 2026-10-12
topics:
  - Cybersecurity
  - Wazuh
  - Projects
  - Detection Engineering
externalUrl: https://github.com/rooshanriaz/Attack-Simulation-using-Wazuh-SIEM
---

## The lab

This project demonstrates a vulnerability assessment and attack-simulation
workflow across a small enterprise-style environment. The goal was to identify
weaknesses in web applications, operating systems, and network services while
using Wazuh as the centralized monitoring and detection layer.

The environment was built from four virtual machines:

- **Kali Linux** as the attacker and assessment platform
- **Ubuntu Server** hosting Apache, PHP, MariaDB, and DVWA
- **Windows Server 2012** hosting IIS and SMB file sharing, including legacy SMBv1
- **Wazuh SIEM Server** for centralized logging, alerting, and event correlation

The target systems ran Wazuh agents so their security and system events could be
forwarded to the SIEM for analysis.

## Architecture

The architecture connected the assessment workstation to two intentionally
vulnerable targets and a central Wazuh monitoring server:

![Attack simulation lab architecture](https://raw.githubusercontent.com/rooshanriaz/Attack-Simulation-using-Wazuh-SIEM/main/VA-Architecture.jpg)

[View the original architecture image in the GitHub repository](https://github.com/rooshanriaz/Attack-Simulation-using-Wazuh-SIEM/blob/main/VA-Architecture.jpg).

## Assessment workflow

The work followed a structured penetration-testing sequence rather than running
tools without a defined objective.

### 1. Reconnaissance and discovery

The first phase used `ping` and `nmap` to identify live hosts, confirm their
addresses, and understand the boundaries of the lab network.

### 2. Enumeration

Detailed Nmap scans were used for port discovery, service-version detection,
operating-system fingerprinting, and banner grabbing. This established the
exposed attack surface before vulnerability testing began.

### 3. Vulnerability scanning

Nmap NSE scripts, Nessus, and manual validation were used to identify and
confirm weaknesses while filtering out false positives. The emphasis was on
understanding which findings were actionable rather than treating scanner output
as a final result.

### 4. Web application testing

DVWA provided a controlled target for testing common web vulnerabilities,
including SQL injection, reflected XSS, command injection, unrestricted file
upload, and CSRF. Directory enumeration also exposed paths such as configuration,
database, documentation, and test resources.

### 5. Selective exploitation

Safe demonstrations were used to validate the impact of selected findings. The
assessment included web application exploit validation, reverse-shell scenarios,
and SMB vulnerability testing with Metasploit against the isolated lab targets.

### 6. Detection and monitoring

During the attack phases, Wazuh collected and correlated logs from the target
systems. The dashboard and alerting workflow provided visibility into activity
that would otherwise be distributed across individual hosts.

## Tools and technologies

The project combined offensive assessment tools with defensive monitoring:

- **Assessment:** Nmap, Metasploit Framework, Gobuster, Nessus, Burp Suite, and custom scripts
- **Monitoring:** Wazuh, Wazuh agents, and the ELK Stack
- **Targets:** DVWA, Apache, IIS, SMB, PHP, and MariaDB

This combination made it possible to compare the action performed by the tester
with the telemetry and alerts produced by the monitored systems.

## Key findings

The assessment documented several high-risk conditions across the target
systems:

- An end-of-life Ubuntu environment and outdated OpenSSH service
- SQL injection, XSS, command injection, unrestricted file upload, and CSRF in DVWA
- Legacy SMBv1 exposure and weak Windows Server configurations
- IIS misconfiguration and missing security patches
- Directory enumeration revealing sensitive application paths
- Weak authentication and insufficient network segmentation

These findings illustrate why vulnerability management needs both technical
validation and continuous monitoring. A vulnerable service is a risk, but the
ability to detect exploitation attempts determines how quickly defenders can
respond.

## What I learned

The project reinforced the relationship between vulnerability assessment and
security operations. Scanning and exploitation show where controls fail; Wazuh
helps show what those failures look like in telemetry.

The most useful workflow was to keep the assessment structured, validate findings
manually, and then check whether the activity was visible in the SIEM. That
creates a practical feedback loop between attack simulation, detection, and
remediation.

The full project report, GRC traceability matrix, README, and architecture asset
are available in the [Attack-Simulation-using-Wazuh-SIEM repository](https://github.com/rooshanriaz/Attack-Simulation-using-Wazuh-SIEM).

## Scope and safety

All testing was performed in a controlled virtual lab against intentionally
vulnerable systems. No production systems, internet-facing targets, or
third-party infrastructure were tested.