# Enterprise Vulnerability Assessment & Posture Analysis (NIST SP 800-115)

## Executive Summary
This project presents an enterprise-grade dual-target vulnerability assessment executing the technical testing methodology outlined in **NIST Special Publication 800-115**. The engagement evaluates and contrasts a legacy, unhardened service environment against a modern, host-hardened server baseline utilizing **Greenbone Vulnerability Management (GVM 25.04.0 / OpenVAS)** and **Nmap**.

## Target Architecture & Engagement Scope
| Endpoint | Operating System | IP Address | Testing Classification |
| :--- | :--- | :--- | :--- |
| **Testing Node** | Kali Linux 64-bit (Kernel 6.x) | 192.168.64.129 | Assessment Originator |
| **Target Node 1** | Metasploitable Linux (Ubuntu 8.04 baseline) | 192.168.64.133 | Legacy Unhardened Environment |
| **Target Node 2** | Microsoft Windows Server 2022 | 192.168.64.130 | Hardened Enterprise Baseline |

## Quantitative Vulnerability Breakdown
* **Total Scanned Findings:** 630 raw detections
* **Actionable Vulnerabilities (QoD ≥ 70%):** 64 findings
* **Severity Distribution:** 12 Critical | 9 High | 37 Medium | 6 Low

| Assessment Metric | Metasploitable Linux (192.168.64.133) | Windows Server 2022 (192.168.64.130) |
| :--- | :--- | :--- |
| **Overall Severity** | **10.0 (Critical)** | **0.0 (Hardened / Filtered)** |
| **Open Ports Detected** | 19 distinct services | 0 unfiltered ports |
| **Associated CVEs** | 32 unique identifiers | 0 vulnerabilities detected |
| **Perimeter Defense** | Unrestricted / Permissive | Host Firewall Drop / State-suppressed |

## Key Technical Vulnerabilities (Target 1)
* **CVE-2011-2523 (vsftpd 2.3.4 Backdoor):** Root shell listener spawned on port `6200/TCP` triggered by specific byte sequence inputs on the FTP daemon.
* **CWE-284 (Ingreslock 1524/TCP Backdoor):** Legacy unauthenticated root listener providing direct administrative command line access.
* **CVE-2004-2687 (DistCC Compilation Daemon RCE):** Arbitrary code and command injection via port `3632/TCP` without client verification.
* **BID-47071 (Distributed Ruby dRuby RCE):** Remote method invocation on port `8787/TCP` allowing arbitrary system call execution.
* **Database Weaknesses:** Unauthenticated root access to MySQL (3306) and default administrative credentials on PostgreSQL (5432).

## Host Hardening Evaluation (Target 2)
* Evaluated Windows Defender Firewall default ingress policy (`Drop` for unsolicited SYN and ICMP requests).
* Validated automated scanner alive-test suppression during black-box scanning sweeps.
* Documented internal auditing prerequisites for credentialed RPC and SMB signing verification behind network perimeter filters.

## Engineering Setup & Troubleshooting
* **Storage Resizing:** Expanded root volume to 100 GB using `swapoff`, `growpart`, and `resize2fs` to accommodate 95,000+ OpenVAS Network Vulnerability Tests (NVTs).
* **Database Collation Fix:** Diagnosed and remediated template collation version mismatches in PostgreSQL using `ALTER DATABASE ... REFRESH COLLATION VERSION`.
* **Credential & Daemon Lifecycle:** Deployed GVM administrative service tokens, established socket binds, and mapped user feed ownership.

## Project Deliverables & Reports
* [Comprehensive VAPT Report (DOCX)](docs/VAPT-NIST-01_kamalpreetSingh.docx)
* [Metasploitable Linux Full Scan Report (PDF)](docs/OpenVAS_Report_Metasploitable.pdf)
* [Windows Server 2022 Full Scan Report (PDF)](docs/OpenVAS_Report_Windows_2022_Server.pdf)
