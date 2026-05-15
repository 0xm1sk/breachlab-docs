# Ghost Track - Lvl 5: Signal in the Noise

You're faced with a "vault" full of hundreds of small files (`record_0001`, `record_0002`, etc.). Opening them one by one would take forever. This is where `grep` becomes your best friend.

The goal is to find the "signal"—the one entry that doesn't fit the pattern.

First, look at a few files to see the "noise":
```bash
cat record_0001
```
It just looks like a timestamp and a random hash. To find the one that's different, you can use `grep` to search through *all* files in the directory at once:

```bash
cat record_* | grep -v "STATUS"
```
The `-v` flag in `grep` is "invert-match." It tells grep: "Show me everything that DOES NOT contain the word STATUS." 

By filtering out the thousands of boring "STATUS" lines, the actual password—which has a different format—pops right out.

**Result:** `REDACTED`

**TL;DR:** When dealing with huge amounts of logs or data, don't read them. Use `grep -v` to filter out the noise and let the secret reveal itself.
