# PHP runtime security hardening checklist

Use this as a defensive workflow for systems you own or are authorized to assess. Consult the official PHP advisory linked below for the current branch-specific release guidance and detailed mitigation notes.

## Before changes

- [ ] Inventory PHP version, SAPI, OS package source, container image digest, and deployment owner.
- [ ] Identify all entry points that serve PHP requests, including worker pools and replicas.
- [ ] Preserve relevant reverse-proxy, web server, PHP runtime, container, and process telemetry.
- [ ] Record a baseline of application behavior and health checks.
- [ ] Prioritize exposed systems and confirm a maintenance/rollback plan.

## Patch and deploy

- [ ] Follow the official PHP advisory and the OS/distribution vendor's supported security update path.
- [ ] Rebuild container images from trusted, patched base images; do not rely only on a host package update if the application bundles PHP inside an image.
- [ ] Deploy patched artifacts to every replica, worker pool, sidecar, and application that uses an affected runtime.
- [ ] Apply configuration-level exposure reduction only according to the official advisory; treat it as defense-in-depth rather than a replacement for patching.
- [ ] Restart the relevant workers/services where required for changes to take effect.

## Verify after deployment

- [ ] Verify the running PHP runtime/package in each environment, not only the source manifest.
- [ ] Confirm all replicas use approved image digests and that stale containers have been replaced.
- [ ] Run application health checks and regression tests for upload/session workflows.
- [ ] Confirm expected logging and monitoring continue to work.
- [ ] Check process telemetry for unexpected child processes and container events during authorized tests.
- [ ] Record before/after evidence, timestamps, version/package state, and the identity of the verifier.

## If exploitation may have occurred

- [ ] Preserve logs, container events, process lineage, egress evidence, file metadata, and image digests before rebuilding.
- [ ] Investigate unexpected web-worker child processes, new files, suspicious access to secrets, and outbound connections.
- [ ] Rotate secrets that may have been exposed, from a trusted environment.
- [ ] Check for persistence and lateral movement; patching alone does not establish that a system is clean.
- [ ] Document scope, containment, remediation, validation, and remaining uncertainty.

## Reporting standard

Every finding should record: **observation → interpretation → confidence → impact → recommendation → validation evidence**. Never label a hypothesis as a detected event without supporting telemetry.
