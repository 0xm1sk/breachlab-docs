# Ghost Track - Lvl 1: First Contact

You've just landed on the box. There's no fancy setup, just a home directory. The goal is simple: find the password for `ghost1`.

The first thing any operator does is look around. Check your current directory:
```bash
ls
```
You'll see a `README` and a `workspace` folder. The `README` is just a note from Kael saying he left in a hurry and didn't clean up. That's basically a hint that the secret is just sitting there in a file somewhere.

Dive into the workspace:
```bash
cd workspace/archive
ls
```
You'll find a file called `credentials`. Give it a `cat`:
```bash
cat credentials
```

**Result:** `REDACTED`

**TL;DR:** Don't overthink the first level. Do a basic directory traversal, look for files that sound like "passwords" or "credentials," and you're in.
