---
title: Building a cyber security capstone from attack to incident response
excerpt: A seven-phase enterprise security lab combining offensive testing, threat modeling, detection engineering, purple teaming, and DFIR.
pubDate: 2026-10-12
topics:
  - Cybersecurity
  - Projects
  - Detection Engineering
  - DFIR
externalUrl: https://github.com/rooshanriaz/Cyber-Capstone
---

## The project

My Cyber Security Capstone is a seven-phase lab designed to follow an enterprise
security lifecycle: identify weaknesses, understand risk, detect adversary
behavior, validate controls, respond to compromise, and recover with better
defenses.

The project connects offensive security with blue team operations instead of
treating them as separate exercises. The work uses MITRE ATT&CK, OWASP ASVS,
OWASP Top 10, NIST SP 800-53, STRIDE, and DREAD to connect technical findings
to security controls and business impact.

The full lab notes and supporting detail are available in the
[Cyber-Capstone repository](https://github.com/rooshanriaz/Cyber-Capstone).

## 1. Offensive security assessment

The first phase assessed a vulnerable CryptoBank trading application from the
perspective of an unauthenticated external attacker. The workflow covered
reconnaissance, service enumeration, web application mapping, vulnerability
discovery, exploitation, post-exploitation analysis, and remediation planning.

The assessment identified a chain of weaknesses including time-based blind SQL
injection, exposed development resources, credential reuse, unsafe command
execution functionality, and privilege escalation opportunities through an
outdated internal service. The important lesson was that the business impact did
not come from one isolated bug. The weaknesses could be chained into full system
compromise.

The phase used Kali Linux, Nmap, Burp Suite, Gobuster, SQLMap, and Nikto. The
findings were mapped to attack techniques and translated into recommendations
covering parameterized queries, authentication, infrastructure hardening,
centralized logging, and WAF protections.

## 2. Threat modeling and risk assessment

The second phase moved from exploitation to secure design. I modeled the
CryptoBank architecture with data flow diagrams, identified trust boundaries,
and analyzed threats using STRIDE.

DREAD scoring was then used to prioritize the threats. SQL injection ranked as
the highest-risk issue because it could expose authentication and financial data,
while exposed development tools and insecure authentication over HTTP also
created high-impact attack paths.

The results were mapped to NIST SP 800-53 and OWASP ASVS controls. This made the
assessment useful beyond a vulnerability list: each threat had a security
requirement, a business impact, and a remediation path that could be carried
into the secure SDLC.

## 3. Adversary emulation

The third phase used the VulnHub DC-9 environment to model an attack campaign
against an intentionally vulnerable target. The exercise followed a controlled
chain of reconnaissance, application enumeration, initial access, credential
access, discovery, lateral movement, and privilege escalation.

Each activity was mapped to MITRE ATT&CK, including active scanning, exploitation
of a public-facing application, credentials in files, service discovery, SSH
remote services, and exploitation for privilege escalation.

The environment was isolated in a host-only VirtualBox network with no internet
or production connectivity. That boundary was part of the exercise, not an
afterthought: safe adversary emulation needs explicit scope, repeatable controls,
and clear detection opportunities.

## 4. Detection engineering

The fourth phase built a threat-informed detection workflow using Wazuh SIEM,
Sysmon, and Atomic Red Team. The pipeline collected endpoint telemetry, applied
custom detection logic, mapped alerts to ATT&CK, and validated the rules with
controlled simulations.

The detection use cases covered:

- LSASS memory access and credential dumping (T1003.001)
- Registry Run Key persistence (T1547.001)
- System information discovery (T1082)
- Obfuscated PowerShell and other obfuscated content (T1027)

The exercise showed why detection engineering needs both telemetry and
validation. A rule is not finished when it is written; it needs a known test,
an expected alert, an ATT&CK mapping, and tuning based on the result.

## 5. Purple team operations

The fifth phase connected adversary emulation to defensive monitoring. Havoc C2
was used to simulate command-and-control activity against a Windows endpoint,
while Sysmon, Wazuh Agent, and Wazuh SIEM provided the defensive view.

The team evaluated callback establishment, payload execution, credential access,
persistence, discovery, and PowerShell obfuscation. The strongest detections
covered LSASS access, registry persistence, and discovery commands. Beaconing and
network-based command-and-control analytics were identified as areas needing
more coverage.

This changed the question from "can the attacker get in?" to "how quickly and
accurately can the defender see what happens next?"

## 6. Control validation and metrics

The sixth phase measured whether the deployed controls were actually effective.
The validation focused on ATT&CK coverage, alert quality, detection latency,
investigation context, and compliance alignment.

The targeted techniques generated alerts with accurate mappings and useful
context. Credential access, persistence, and discovery coverage were assessed as
strong, while defense-evasion coverage was more moderate. The gaps pointed to
future work around command-and-control analytics, lateral movement, privilege
escalation, network telemetry, and threat hunting.

The assessment also mapped the work to NIST Cybersecurity Framework functions and
NIST SP 800-53 controls including audit review, system monitoring, incident
monitoring, input validation, and least privilege.

## 7. Digital forensics and incident response

The final phase investigated a simulated Windows endpoint compromise involving
a Sliver-based implant and credential theft activity. The investigation followed
an evidence-first workflow:

```text
Identification -> Acquisition -> Preservation -> Analysis
-> Timeline Reconstruction -> IOC Extraction -> Response Recommendations
```

FTK Imager was used for evidence acquisition, Volatility 3 for memory analysis,
Autopsy and The Sleuth Kit for disk and timeline analysis, and SHA-256 hashing
for evidence integrity verification.

Correlating memory, disk, process, network, and log artifacts reconstructed the
attack path from payload execution to command and control, credential dumping,
and post-compromise activity. The resulting response recommendations covered
containment, eradication, recovery, credential resets, host rebuilding, and new
detections for beaconing, suspicious process ancestry, and credential dumping.

## What the capstone demonstrated

Across the seven phases, the project demonstrated how the security lifecycle can
work as one connected program:

- Offensive testing exposed weaknesses and realistic attack paths.
- Threat modeling connected architecture to risk and control requirements.
- Detection engineering turned ATT&CK behaviors into testable analytics.
- Purple team exercises measured visibility against adversary activity.
- Control validation made detection quality and gaps measurable.
- DFIR connected evidence to timelines, IOCs, impact, and response decisions.

The strongest takeaway was that security work becomes more useful when every
finding leads somewhere: a control, a detection, a metric, a response action, or
a design change.

## Scope and safety

All testing, simulations, and forensic activity were performed in isolated,
authorized academic lab environments against intentionally vulnerable systems.
No production systems, internet-facing targets, or third-party infrastructure
were tested.