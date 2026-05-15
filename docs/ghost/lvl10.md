# Ghost Track - Lvl 10: Noise Floor

The final level of this set is a test of your binary analysis skills. You're given a file called `signal.bin` that is mostly random noise.

If you `cat` it, you'll see garbage. The goal is to find the only "readable" string buried inside the binary.

The `strings` command is designed exactly for this. It scans a binary file and prints every sequence of printable characters it finds.
```bash
strings signal.bin
```
The output will be a long list of random strings. To find the flag, you can pipe it into `grep` and look for a pattern (like the word "SHARD" or just a long alphanumeric string):
```bash
strings signal.bin | grep -E "[A-Za-z0-9]{5,}"
```
By filtering for strings longer than 5 characters, the "signal" stands out from the "noise."

**Result:** `REDACTED`

**TL;DR:** When a file looks like binary garbage, use `strings`. It strips away the non-printable junk and reveals the hidden text.
