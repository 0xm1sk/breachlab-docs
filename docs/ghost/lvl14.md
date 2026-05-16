# Level 14: Port 30000

A service is running on port 30000, and it's willing to trade the next password for the current one. Simple enough, but there's a catch: you can't use a browser or any high-level client library.

This is where you have to go raw. You need a tool that just opens a TCP pipe and lets you type. `netcat` (`nc`) is the industry standard for this.

I just connected to the local loopback address on the specified port and sent the password I got from Level 13.

```bash
nc 127.0.0.1 30000
# Input: REDACTED
```
The service validates the password and immediately spits out the next one.

**Result:** `REDACTED`

**TL;DR:** When you need to talk to a raw TCP port without a browser, `nc` is your best friend.
