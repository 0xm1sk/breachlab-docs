# Level 13: Key Not Password

You hit a wall here: there is no password for the next level. Instead, you're given a private SSH key.

If you've never dealt with keys before, it can feel weird because the login process is different. You aren't providing a secret *string* to the server; you're providing a *file* that proves you are the authorized user.

The trick is the `-i` flag in SSH. It stands for "identity." By using it, you're telling the SSH client: "Don't ask for a password, use this specific key file to authenticate me."

```bash
ssh -i sshkey.private ghost13@localhost
```
Once you're in, the password for the next level is just sitting there in the flag.

**Result:** `REDACTED`

**TL;DR:** When there's no password, look for a private key and use `ssh -i <keyfile>` to authenticate.
