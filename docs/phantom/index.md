# Phantom Track: Post-Exploitation & Tradecraft

Ghost ended at “you got a shell.” Phantom starts there and does not stop until the operation is complete. This is the complete chain a real operator runs against a real compromised environment in 2026.

## Core Competencies

The objective of this track is to master the full post-foothold discipline. By graduation, you will be able to:

**Privilege Escalation**
Identify and exploit every realistic escalation path in under ten minutes, including SUID/GTFOBins, sudo misconfigurations, capabilities, and kernel CVEs.

**Credential Harvesting**
Extract secrets from memory dumps, history files, config files, environment variables, and service tokens.

**Persistence & Evasion**
Install persistence that survives reboots (SSH, cron, systemd, PAM) and operate invisibly by bypassing auditd and utilizing LOLBins.

**Lateral Movement**
Pivot through multi-segment networks using SSH tunnels, ligolo-ng, and covert channels.

**Container & Cloud Escape**
Detect container runtimes, escape via Docker sockets or privileged modes, take over Kubernetes clusters via RBAC abuse, and pivot into cloud infrastructure via IMDS.

**Operational Cleanup**
Exfiltrate data through covert channels (DNS, HTTPS, ICMP) and wipe every forensic artifact across a multi-host operation.

---

## Level Index

### I. Escalation (0–9)
- [Level 9: Stack Day](./lvl9.md) — **Precision Overflow & ASLR Bypass**

---

**Who this is for:** Operatives who have finished Ghost or can operate on a fresh Linux box without hesitation. Phantom assumes you live in a shell; it does not teach you how to move files—it teaches you how to own the environment.
