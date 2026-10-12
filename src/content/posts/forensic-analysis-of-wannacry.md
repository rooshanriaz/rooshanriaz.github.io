---
title: Forensic analysis of a WannaCry ransomware attack
excerpt: Reconstructing ransomware activity through disk and memory forensics in an isolated Windows 10 lab.
pubDate: 2026-10-12
topics:
  - DFIR
  - Digital Forensics
  - Projects
externalUrl: https://github.com/rooshanriaz/Forensic-Analysis-of-Ransomware-Attack
---

## The investigation

This project examined a simulated WannaCry ransomware attack in an isolated
VMware environment. The investigation focused on two questions: what artifacts
did the attack leave behind, and how could disk and memory evidence be combined
to explain the malware's behavior?

The work was carried out on a Windows 10 analysis environment with controlled
VMware networking. Keeping the lab isolated made it possible to handle the
sample and observe its effects without exposing production systems or external
infrastructure.

The complete project report is available in the
[Forensic-Analysis-of-Ransomware-Attack repository](https://github.com/rooshanriaz/Forensic-Analysis-of-Ransomware-Attack).

## Evidence and tools

The investigation used separate acquisition and analysis steps:

- **VMware Workstation** for the isolated host-only and NAT lab networks
- **FTK Imager** for disk and memory image acquisition
- **Autopsy** for file-system and disk examination
- **Volatility** for memory forensics and process analysis
- **Pestudio** for static malware inspection
- **VirusTotal** for multi-vendor validation of suspicious artifacts

Separating acquisition from analysis helped preserve the evidence and made the
investigation repeatable. The disk image provided a durable record of files and
encrypted artifacts, while memory analysis exposed activity that would disappear
after shutdown.

## Disk forensics

Autopsy was used to inspect the disk image and identify traces associated with
the ransomware infection. The analysis located `WannaDecryptor.exe` alongside
encrypted files and other artifacts that helped establish the presence of the
malware on the system.

Disk analysis was useful for answering questions such as:

- Which suspicious executables were written to the system?
- Which files showed signs of encryption or ransomware activity?
- What artifacts could be connected to the infection timeline?

These findings formed the persistent-evidence layer of the investigation. They
could be revisited and correlated with volatile artifacts from memory.

## Memory forensics

Volatility was used to examine the memory image and track suspicious execution.
The investigation identified a malicious process named `or4qtckT.exe` and
examined its behavior, process context, and in-memory characteristics.

Memory analysis also helped identify self-modifying sections and behavior
consistent with a Trojan. This was an important complement to the disk findings:
the file on disk showed what had been written, while memory showed what was
running and how the system was behaving during the incident.

## Malware validation

Pestudio and VirusTotal were used to validate the suspicious sample and compare
the local evidence with broader malware intelligence. The repository records
that 24 of 72 VirusTotal vendors flagged the sample, including a detection named
`Trojan.GenericKD.76211654`.

Vendor results were treated as supporting evidence rather than the entire
verdict. The strongest conclusion came from combining the vendor signals with
the disk artifact, the suspicious process, encrypted files, and observed malware
behavior.

## Findings

The investigation established several useful indicators and behaviors:

- `WannaDecryptor.exe` was present in the disk image.
- Encrypted files provided evidence of ransomware impact.
- `or4qtckT.exe` appeared as a suspicious running process in memory.
- Self-modifying sections and Trojan-like characteristics were observed.
- External malware intelligence supported the local forensic findings.

Together, these artifacts formed a defensible picture of ransomware activity
without relying on a single filename or scanner result.

## Lessons learned

Ransomware investigations benefit from a layered approach. Disk evidence can
show what survived on the system, memory can reveal active processes and
volatile behavior, and malware-intelligence services can provide useful context.

The project reinforced three practical habits:

1. Preserve evidence before analysis.
2. Correlate disk, memory, and malware-validation results.
3. Treat tool output as evidence to interpret, not a substitute for an
   investigation.

The result is a clearer path from artifact discovery to incident understanding,
containment decisions, and future detection improvements.

## Scope and safety

This analysis was performed in an isolated academic VMware environment for
educational and defensive purposes. No production systems, internet-facing
targets, or third-party infrastructure were involved.