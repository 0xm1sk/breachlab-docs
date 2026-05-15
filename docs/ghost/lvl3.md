# Ghost Track - Lvl 3: In The Shadows

This level is all about "hidden" files. In Linux, any file or folder that starts with a dot (`.`) is hidden from a standard `ls` command.

First, check the home directory:
```bash
ls -a
```
The `-a` flag tells `ls` to "show all," including the hidden ones. You'll see a `.memo` file. Reading it gives you a hint: *"The work happens off the main paths."*

Now head into the `investigation` folder:
```bash
cd investigation
ls -la
```
Again, use `-a` to see the hidden folders. You'll find a folder called `.leads`. Dive in:
```bash
cd .leads
ls -a
```
Inside `.leads`, you'll find more hidden files like `.source_alpha`, `.source_beta`, etc. Since they all look similar, you can just `cat` all of them at once using a wildcard:
```bash
cat .*
```

**Result:** `REDACTED`

**TL;DR:** If you don't see anything in a folder, always use `ls -a`. Secrets are almost always hidden in dot-files (`.secret`) or dot-directories (`.hidden/`).
