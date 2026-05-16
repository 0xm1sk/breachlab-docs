# Level 18: No Shell For You

This level is a bit of a prank. You log in via SSH, and the very second you arrive, the server runs a startup script that boots you. Connection closed. No interactive shell, no `ls`, nothing.

It feels like you're locked out, but there's a loophole. SSH doesn't actually *require* an interactive shell to work. You can tell SSH to execute a specific command on the remote server and then exit immediately.

Since the server kicks you out *after* the initial command is processed, you can just bypass the shell entirely and tell the server to `cat` the flag for you.

```bash
ssh ghost17@204.168.229.209 -p 2222 'cat flag'
```
The server executes the command, spits out the password, and then kicks you out. Mission accomplished.

**Result:** `REDACTED`

**TL;DR:** If a server kicks you on login, use SSH to execute a single command (`ssh user@host 'command'`) to bypass the interactive shell requirement.
