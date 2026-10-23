# php-runtime-security-study
Purple Hat lab notes: PHP runtime security, vulnerability analysis, defensive detection, and remediation
# PHP Runtime Security Study — Purple Hat Labs

## Overview

This project documents my learning from an authorized cybersecurity training lab focused on PHP runtime security and memory-safety concepts.

My objective is to understand how runtime-level vulnerabilities affect web applications and how offensive testing can inform detection, incident response, and remediation.

## Learning objectives

- Understand the relationship between PHP runtime behavior and application security.
- Study memory-safety concepts and the role of ASLR.
- Practice evidence-based investigation and Linux troubleshooting.
- Distinguish observed evidence from assumptions.
- Connect offensive security concepts with defensive monitoring and patch validation.

## Lab environment

- Platform: TryHackMe
- Environment: Ubuntu host with a local PHP/Apache service running in a container
- Initial investigation: HTTP response analysis and service identification
- Tools practiced: SSH, Bash, curl, grep, sed, Linux file-permission utilities

## Defensive analysis

My defensive study focuses on:

- Maintaining an accurate inventory of PHP versions.
- Applying vendor security updates.
- Reviewing unusual web requests and session-related behavior.
- Considering available application, server, and container telemetry.
- Validating mitigations in an authorized test environment.

These are investigation and detection areas, not claims that every signal was observed during the lab.

## Key lessons

- Verify environment assumptions before testing.
- Read scripts and understand command-line arguments before execution.
- Diagnose permission problems using evidence.
- Distinguish the host filesystem from container filesystems.
- Record only findings supported by observed output.
- Treat configuration changes as potential mitigations, not substitutes for vendor patches.

## Scope and responsible use

These notes describe my learning and defensive analysis. This public repository intentionally excludes flags, answers, room-specific exploit code, and step-by-step solutions for active training content.

All testing was intended for an authorized training environment.

## Future work

- Develop generic detection hypotheses and document their limitations.
- Practice log analysis and incident timelines.
- Build a patch-verification checklist.
- Apply the same offensive-to-defensive methodology to additional labs.

---

**Learning path:** Purple Hat Cybersecurity — offensive understanding, defensive validation, and security engineering.

