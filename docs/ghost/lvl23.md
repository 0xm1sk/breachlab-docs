# Ghost Track - Lvl 23: Graduation

This is the final run. You've got three shards and you need to feed them to a gatekeeper on port `31339`.

When you see multiple weird files like this, don't panic. Just do some "triage." Look at the file extensions and pick the tool that fits the format.

**Shard 1 (`relic.bin`):**
It's a `.bin` file. If you `cat` it, your terminal will probably freak out and show you a bunch of weird symbols. That's because it's binary data. To find human-readable text inside that mess, use `strings`. It just scans the file for sequences of characters that look like actual words.
```bash
strings relic.bin | grep "SHARD"
```
**Result:** `REDACTED`

**Shard 2 (`scroll.b64`):**
The `.b64` is a dead giveaway. It's just Base64 encoding. This isn't encryption—it's just a way to represent data. You can reverse it instantly:
```bash
base64 -d scroll.b64
```
**Result:** `REDACTED`

**Shard 3 (The SUID Helper):**
Now you're out of files in your home folder. Now you ask: *Is there any tool on this whole system that has more power than I do?* 

Search for SUID binaries. These are special files that run with the permissions of the owner (root), no matter who starts them. It's a classic way to escalate privileges.
```bash
find / -perm -4000 2>/dev/null | grep ghost22
```
I found `/usr/local/bin/ghost-archivist`. Since it's SUID and called an "archivist," it's probably designed to access archived secrets. Just execute it:
```bash
/usr/local/bin/ghost-archivist
```
**Result:** `REDACTED`

**The Finish Line:**
Now just pipe them all together using `nc` (netcat) in the exact format the `BRIEFING` asked for. If you get a "Connection refused" or similar, make sure you're hitting the right port.
```bash
nc localhost 31339
# Then send: SHARD1:REDACTED|SHARD2:REDACTED|SHARD3:REDACTED
```

**Graduation Flag:** `REDACTED`

**TL;DR:** `.bin` $\rightarrow$ `strings`, `.b64` $\rightarrow$ `base64`, and if you're stuck $\rightarrow$ `find / -perm -4000` to find some SUID magic.
