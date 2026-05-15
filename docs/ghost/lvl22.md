# Ghost Track - Lvl 22: Digging through Git

You find a repo, check `config.txt`, and the secret is just `***`. It looks like the dev deleted the key and committed the change, thinking it's gone.

But here's the thing: Git is a time machine. The "current" version you see is just the tip of the iceberg. Everything that ever existed in that repo is still there, hidden in the history.

First, check the tags. Tags are like bookmarks for specific versions (like a release).
```bash
git tag -l
```
I saw `v0.9-internal`. Whenever you see "internal," "debug," or "dev" in a version name, that's where the gold is. Developers almost always hardcode passwords when they're testing things.

Now, don't just guess. Use the logs to see what actually happened in that version:
```bash
git log --all
```
I found a commit message: `"temp: hardcode prod secret for debug trace"`. That is basically a neon sign saying "The flag is right here."

Now, the final step. You don't need to checkout the whole branch; you just need to see the **diff**—the exact changes made in that specific commit:
```bash
git show v0.9-internal
```
Look for the lines starting with `+`. That's the text that was added. That's your secret.

**Result:** `REDACTED`

**TL;DR:** If it's a Git repo, the current version is a lie. Dig through the tags and commit logs for "debug" or "temp" commits.
