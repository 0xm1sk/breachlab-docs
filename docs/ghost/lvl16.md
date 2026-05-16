# Level 16: Port Range

The target is out there, but it's hidden. One port in a wide range is speaking TLS, and the rest are either closed or dead silent.

You could spend all day guessing ports, but that's not how an operator works. You map the attack surface first. I used `nmap` to scan all ports on the local machine to see what was actually listening.

```bash
nmap -p- localhost
```
The scan reveals a few open ports. I didn't know which one was the right one, so I just looped through them using `openssl s_client` until I found the one that actually requested a password. Port 31790 turned out to be the goldmine.

```bash
openssl s_client -quiet -connect 127.0.0.1:31790
# Input: REDACTED
```

**Result:** `REDACTED`

**TL;DR:** Don't guess ports. Scan the host with `nmap`, identify open services, and probe them one by one.
