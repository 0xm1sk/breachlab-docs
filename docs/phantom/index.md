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
- [Level 0: Entry Point](./lvl0.md)
- [Level 1: SUID Basics](./lvl1.md)
- [Level 2: Sudo Rules](./lvl2.md)
- [Level 3: Library Hijacking](./lvl3.md)
- [Level 4: Capabilities](./lvl4.md)
- [Level 5: Writable Files](./lvl5.md)
- [Level 6: Cron & Systemd](./lvl6.md)
- [Level 7: Polkit Abuse](./lvl7.md)
- [Level 8: Ptrace Injection](./lvl8.md)
- [Level 9: Stack Day](./lvl9.md)

### II. Harvest & Persist (10–15)
- [Level 10: Memory Dumps](./lvl10.md) — *Coming Soon*
- [Level 11: Token Hunting](./lvl11.md) — *Coming Soon*
- [Level 12: SSH Persistence](./lvl12.md) — *Coming Soon*
- [Level 13: PAM Backdoors](./lvl13.md) — *Coming Soon*
- [Level 14: Auditd Bypass](./lvl14.md) — *Coming Soon*
- [Level 15: Timestomping](./lvl15.md) — *Coming Soon*

### III. Lateral Movement (16–19)
- [Level 16: SSH Tunneling](./lvl16.md) — *Coming Soon*
- [Level 17: Ligolo-ng](./lvl17.md) — *Coming Soon*
- [Level 18: Internal Recon](./lvl18.md) — *Coming Soon*
- [Level 19: Pivot Chain](./lvl19.md) — *Coming Soon*

### IV. Container & Cloud (20–26)
- [Level 20: Container Detection](./lvl20.md) — *Coming Soon*
- [Level 21: Docker Escape](./lvl21.md) — *Coming Soon*
- [Level 22: Leaky Vessels](./lvl22.md) — *Coming Soon*
- [Level 23: K8s Pod Escape](./lvl23.md) — *Coming Soon*
- [Level 24: Cluster Takeover](./lvl24.md) — *Coming Soon*
- [Level 25: IMDS Pivot](./lvl25.md) — *Coming Soon*
- [Level 26: Cloud IAM](./lvl26.md) — *Coming Soon*

### V. Operations (27–31)
- [Level 27: Custom Tooling](./lvl27.md) — *Coming Soon*
- [Level 28: DNS Exfil](./lvl28.md) — *Coming Soon*
- [Level 29: HTTPS/ICMP Channels](./lvl29.md) — *Coming Soon*
- [Level 30: Multi-Host Cleanup](./lvl30.md) — *Coming Soon*
- [Level 31: Graduation Mission](./lvl31.md) — *Coming Soon*

---

**Who this is for:** Operatives who have finished Ghost or can operate on a fresh Linux box without hesitation. Phantom assumes you live in a shell; it does not teach you how to move files—it teaches you how to own the environment.
