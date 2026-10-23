**PHP Runtime Security Study — Purple Hat Lab Journal**
A portfolio-oriented study of PHP runtime security, memory-safety risk, Linux troubleshooting, and defensive validation, informed by an authorized TryHackMe exercise and the PHP team's public security advisory issued on 21 October 2026.
Public portfolio edition: This repository focuses on concepts, observed evidence, defensive reasoning, and remediation. It intentionally excludes room flags, answer strings, room-specific exploit code, and a step-by-step solution.

Project overview
I am developing a Purple Hat workflow: understand offensive concepts in an authorized lab, record verifiable evidence, map plausible attacker behavior to defensive telemetry, and recommend remediation that can be validated.
What this project demonstrates
- Runtime-level vulnerabilities can be reachable before application-level validation or route logic.
- Memory-safety failures and unsafe error-path cleanup can affect process integrity.
- ASLR makes address prediction harder but does not fix the underlying vulnerability.
- HTTP response metadata can support asset inventory, but a header alone does not prove vulnerability or exploitation.
- Host and container filesystems, users, and permissions must be verified rather than assumed.
- Findings should separate captured evidence from assumptions, user-reported outcomes, and untested detection hypotheses.
- Runtime patching is the primary remediation; configuration and network controls are supplemental unless the vendor explicitly documents them as a complete fix.
Lab environment
- Platform: TryHackMe
- Environment: Ubuntu lab host with a local PHP/Apache service running in a container
- Tools practiced: SSH, Bash, curl, less, grep, sed, ls, and Linux file-permission checks
Evidence and limitations
The recorded session confirmed that an SSH shell was available, the local HTTP endpoint returned a successful response with technology-identification headers, and a host-side output directory had service-account ownership and restrictive write permissions for other users. It also captured a permission-denied error and verified that a file written with elevated privileges could later be read on the host.

The learner reported completing the room, but the conversation did not preserve a successful exploit proof line, a verified identity from the exploited process, or the exact flag. This portfolio therefore does not claim independent proof of remote command execution and does not invent a flag.
Purple Hat perspective
Offensive understanding
Study how a runtime-level flaw may be reached during request processing before application code runs, and why safe error handling and memory management matter.
Defensive validation
- Inventory PHP versions, SAPIs, container images, and all running replicas.
- Follow the official PHP and distribution-vendor security guidance.
- Review available web, PHP, process, container, filesystem, and egress telemetry when investigating suspected exploitation.
- Treat detection ideas as hypotheses until tested against representative logs and benign baselines.
- Verify the patched version in every deployed runtime; do not assume that patching the host also patches a bundled container runtime.
No packet captures, PHP/Apache logs, process-lineage records, or container-audit records were collected in this session, so detection items are recommendations for future practice rather than confirmed findings.
Project files
- [Public PHP2Shell Purple Hat lab report](./PHP2Shell_Purple_Hat_Report_Public.docx)
- [Download the project bundle (Markdown notes, source list, and checklists)](./php-runtime-security-study-package.zip)
References
- PHP security advisory archive — 21 October 2026
- PHP 8.4.27 release announcement
- TryHackMe Acceptable Use Policy
Re-check vendor advisories and the platform's current policy before reusing or expanding these notes.
Learning path: Purple Hat cybersecurity — offensive understanding, evidence-driven investigation, defensive validation, and security engineering.
