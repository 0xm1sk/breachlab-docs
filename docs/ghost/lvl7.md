# Ghost Track - Lvl 7: Ghost in the Machine

This level is about **Environment Variables**. These are values stored in the shell's memory that programs use for configuration (like API keys or Database URLs).

Start by listing all current environment variables:
```bash
env
```
You'll see a massive list. Look for anything that looks like a secret. I spotted `API_DIGEST=M252X0wzNGtzXzN2M3J5dGgxbmc=`.

That string ends in `=` and looks like random characters—this is a classic sign of **Base64 encoding**. Base64 isn't encryption; it's just a way to represent binary data as text.

To decode it:
```bash
echo "M252X0wzNGtzXzN2M3J5dGgxbmc=" | base64 -d
```

**Result:** `REDACTED`

**TL;DR:** Always check `env` when you land on a box. Developers often leave "secrets" in environment variables, and if they look like random gibberish, try `base64 -d`.
