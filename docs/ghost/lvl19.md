# Level 19: Wrong User

You're on the system, but you're the wrong user. There's a binary that belongs to `ghost19`, and you need to run it to get the password.

The problem? You don't have the permissions to just run another user's private files. But then you notice something in the permission bits. This is the classic **SUID (Set User ID)** trick.

When a file has the SUID bit set, it doesn't run with the permissions of the person who launched it—it runs with the permissions of the file's *owner*.

I used `find` to hunt for any binary owned by `ghost19` that had the SUID bit (`-perm 4000`) set across the entire system.

```bash
find / -user ghost19 -perm 4000 2>/dev/null
```
It pointed me to `/usr/local/bin/ghost-reader`. I executed it, and because of that SUID bit, the program ran as `ghost19` and gave me the password.

**Result:** `REDACTED`

**TL;DR:** Look for SUID binaries owned by the target user to escalate your privileges and access restricted data.
