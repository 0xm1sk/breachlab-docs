# Level 17: Diff

You're given two files, `passwords.new` and `passwords.old`. They look almost identical—thousands of lines of strings. The password is the only single line that differs between them.

If you try to read these by eye, you're going to go insane. Even a small mistake in scanning will leave you stuck for hours. This is the exact scenario the `diff` utility was designed to solve.

Instead of looking for the password, I just asked the system to show me what changed.

```bash
diff passwords.new passwords.old
```
The output highlights the exact line that is different in the new file compared to the old one. No scanning, no guessing.

**Result:** `REDACTED`

**TL;DR:** When comparing two massive datasets for a single change, `diff` is the only way to stay sane.
