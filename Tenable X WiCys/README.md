# Tenable CTF — Writeup

**Platform:** Tenable One / Nessus  
**Category:** Security Operations, Vulnerability Management, Exposure Management  
**Difficulty:** Beginner to Intermediate

\---

## Overview

This CTF was built entirely around the Tenable product ecosystem, requiring participants to navigate multiple platforms and understand how they interconnect. Rather than traditional exploitation challenges, this CTF tested real-world blue team skills — the kind of investigative work a security analyst or engineer would perform daily in an enterprise environment.

Flags were hidden throughout scan data, debug logs, knowledge base entries, plugin outputs, and cloud dashboards — often encoded or obfuscated in creative ways including Base64 and leet speak.

I placed 5th out of the 50 particpants selected to compete.

\---

## Products and Platforms Encountered

### Tenable Nessus

The foundation of several challenges. Participants were required to:

* Import encrypted scan databases (`.db` files) using passwords
* Navigate the Nessus web interface to analyse scan results
* Understand plugin families including **Artificial Intelligence**, **Settings**, **Misc**, and **General**
* Work with scan policies, audit trails, and host-level views

### Tenable Vulnerability Management (Tenable.io)

The cloud-hosted VM platform featured heavily, including:

* The **Findings** explorer with advanced filtering by Plugin ID, severity, and asset
* **Asset inventory** and asset detail views
* **Patch Report** plugin analysis
* Understanding VPR (Vulnerability Priority Rating) vs CVSS scoring

### Tenable Web App Scanning (WAS)

Web application security challenges required navigating:

* WAS-specific plugins covering session management, network timeouts, and secret disclosure
* The **Applications** view within WAS
* Finding proof/evidence sections for web vulnerabilities including XXE, SQL injection, and SSTI
* Understanding the difference between the global Findings explorer and the WAS-specific scan view

### Tenable OT Security

Operational Technology challenges introduced participants to:

* OT asset inventory including PLCs, HMIs, and controllers
* Device properties including firmware versions, open ports, and backplane information
* Understanding OT-specific asset classes and criticality ratings
* The Rockwell/Allen-Bradley ecosystem

### Tenable Identity Exposure (Active Directory)

Identity-focused challenges covered:

* Active Directory misconfiguration detection
* Detection Sub Categories for AD findings
* Kerberos delegation issues and their security implications
* Understanding how AD misconfigs create attack paths

### Tenable One — Exposure Management

The higher-level platform tied everything together:

* **Global Exposure Card** — understanding CES (Cyber Exposure Score) and what drives it
* **Inventory** — cross-product asset search and filtering
* **Attack Path Analysis** — Query Builder, path visualization, and MITRE ATT\&CK technique mapping
* **Exposure Signals** — custom and built-in signals for coverage gap analysis
* **AI Exposure (TAI)** — Tenable's AI security monitoring platform for detecting prompt injection, jailbreaks, and harmful content in AI interactions

\---

## Key Techniques Required

### Navigating Nessus Debug Logs

Many flags were hidden not in the main plugin output but in the **host-level debug logs** — accessible only by clicking through to the individual host view rather than the vulnerability group view. This required understanding how Nessus stores scan data hierarchically.

### Knowledge Base (KB) Analysis

Several challenges required downloading and analysing the raw Knowledge Base data for a scanned host. The KB stores intermediate scan data that plugins write and read from each other — including authentication status, installed software, and error codes. Flags were sometimes Base64 encoded within KB entries.

### Understanding Plugin Dependencies

Some challenges involved plugins that failed to fire because a prerequisite KB key was missing. Understanding the dependency chain — why a vulnerability check didn't run — required using the Audit Trail to trace the execution path.

### False Positive Analysis

A recurring theme was distinguishing genuine vulnerabilities from false positives, including:

* OS-managed packages with backported security fixes
* Version detection based on metadata filenames rather than actual package content
* The significance of `Potential Vulnerability: Managed` in plugin output

### Attack Path Analysis

Using Tenable One's Query Builder to trace attack paths between specific source and destination assets, then identifying the MITRE ATT\&CK technique used at each step of the path.

### AI Security Concepts

Later challenges introduced AI-specific security topics including:

* Prompt injection detection
* Jailbreak attempts
* AI Training Data Poisoning (Runtime Data Manipulation)
* Coverage gap analysis for AI platforms
* Session-level investigation of AI interactions

\---

## Key Lessons

* **Debug logs contain more information than plugin output** — always check the host-level view
* **The KB is the scanner's memory** — understanding what gets stored there explains why plugins behave the way they do
* **Version detection is imperfect** — scanners rely on self-reported metadata which can be stale or misleading
* **Tenable One unifies multiple products** — understanding how VM, WAS, OT, Identity, and AI Exposure feed into a single exposure score is essential for holistic risk management
* **Attack paths reveal compounding risk** — a low-privilege user with a misconfigured ACE can become a domain admin through a chain of individually minor issues

\---

## Tools Used

* Tenable Nessus (web interface)
* Tenable.io / Tenable One (cloud)
* SQLCipher (command line)
* Browser developer tools and Ctrl+F for searching plugin output
* Base64 decoding utilities
* curl for manual web application testing

\---

*This CTF was an excellent hands-on introduction to the Tenable product ecosystem, covering the full breadth of modern enterprise security tooling from traditional vulnerability scanning to cutting-edge AI security monitoring.*

