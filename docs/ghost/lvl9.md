# Ghost Track - Lvl 9: Something's Running

This level is about the `/proc` directory. In Linux, `/proc` is a "virtual" filesystem that gives you a real-time look at the kernel and running processes.

First, find the process you're looking for using `ps`:
```bash
ps aux | grep ghost8
```
You'll see a bunch of python processes. One of them is running as root or has interesting parameters. Each process has a **PID** (Process ID), which is the number in the second column (e.g., `1368726`).

Every process has its own folder in `/proc` named after its PID. Inside that folder, the `environ` file contains the environment variables that specific process is using.

Check the environment of the target PID:
```bash
cat /proc/1368726/environ
```
*(Note: The output of `environ` is null-delimited, so it might look messy, but the secrets are all there.)*

**Result:** `REDACTED`

**TL;DR:** If a secret is in a running process but not in your shell's `env`, go to `/proc/[PID]/environ`. It's the ultimate way to peek into a process's memory.
