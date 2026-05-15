# Ghost Track - Lvl 6: The Listener

In this level, the secrets aren't on the disk anymore—they're running in memory as services. To find them, you need to see what "ports" are open on the machine.

First, scan the local machine to see what's listening:
```bash
nmap -p- localhost
```
You'll see a bunch of open ports in the 30000 range. To talk to these ports, we use `nc` (Netcat), the "Swiss Army knife" of networking.

If you try a random port, you might get a "Wrong password" message. But if you try `nc localhost 30100`, the service tells you: *"Authentication token: GHOST. Secure channel: port 30101."*

Now you just follow the instructions and connect to the other port:
```bash
nc localhost 30101
# Then type: GHOST
```

**Result:** `REDACTED`

**TL;DR:** If there's nothing on the disk, check the ports. `nmap` finds the open doors, and `nc` lets you walk through them.
