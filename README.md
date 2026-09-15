# Enterprise Vulnerability Assessment & Posture Analysis (NIST SP 800-115)

## Executive Summary
This engagement conducts a dual-endpoint vulnerability assessment adhering to **NIST SP 800-115** technical testing methodologies. The assessment contrasts an unhardened legacy daemon environment against an enterprise-hardened baseline to demonstrate perimeter filtering, host configuration divergence, and automated vulnerability validation using **Greenbone Vulnerability Management (GVM 25.04.0 / OpenVAS)** and **Nmap**.

## Target Architecture & Scope
| Endpoint | Role / OS | IP Address | Assessed Posture |
| :--- | :--- | :--- | :--- |
| **Testing Node** | Kali Linux (Kernel 6.x) | 192.168.64.129 | Assessor Node |
| **Target Node 1** | Metasploitable Linux (Ubuntu 8.04) | 192.168.64.133 | Critical Risk (CVSS 10.0) |
| **Target Node 2** | Windows Server 2022 Baseline | 192.168.64.130 | Hardened Perimeter (CVSS 0.0) |

## Findings & Severity Distribution
* **Total Actionable Vulnerabilities Detected (QoD ≥ 70%):** 64
* **Severity Breakdown:** 12 Critical | 9 High | 37 Medium | 6 Low

### Critical Vulnerabilities Highlights
* **CVE-2011-2523 (vsftpd 2.3.4 Backdoor):** Remote root bind shell triggered via malicious smiley byte sequence on port 6200/TCP.
* **CVE-2004-2687 (DistCC Daemon RCE):** Unauthenticated arbitrary command execution leading to direct daemon shell access.
* **BID-47071 (Distributed Ruby dRuby RCE):** Syscall injection exposing remote code execution over port 8787/TCP.
* **Host Hardening Contrast:** Validated Windows Server 2022 default host-based firewall behaviors (unsolicited ICMP drop and perimeter probe rejection).

## Environment Setup & Engine Remediation
* Partition extension and contiguous block alignment via `growpart` and `resize2fs` to accommodate 95,000+ NVTs.
* Direct PostgreSQL collation version alignment (`ALTER DATABASE ... REFRESH COLLATION VERSION`) resolving template table initialization failures.
* Multi-component daemon configuration: `gvmd`, `ospd-openvas`, `gsad`, and Redis socket communication.

## Deliverables
* [Full Assessment Report (DOCX)](docs/VAPT-NIST-01_kamalpreetSingh.docx)
* [Full Assessment Report (PDF)](docs/VAPT-NIST-01_kamalpreetSingh.pdf)
