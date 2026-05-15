# Ghost Track - Lvl 2: Name Game

In this level, Kael is trying to mess with you by naming files in a way that makes the shell act weird. If you try to `cat` a file that starts with a dash or has a space in it, the shell might think you're trying to pass an argument (like `--help`) instead of a filename.

Check the directory:
```bash
ls
```
You see a file called `MANIFEST` and another called `'file name'`. 

If you try to `cat` a file that starts with a dash (like if there was a file named `-secret`), the command would fail. To fix this, you can use the absolute path (like `cat ./-secret`) or the `--` delimiter.

But here, the main obstacle is the space in `file name`. If you just type `cat file name`, Linux thinks you're trying to read two different files: one named `file` and one named `name`.

To handle spaces, you have two choices:
1. **Quoting**: `cat "file name"`
2. **Escaping**: `cat file\ name`

**Result:** `REDACTED`

**TL;DR:** When filenames have spaces or start with dashes, the shell gets confused. Use quotes or backslashes to tell the shell it's one single filename.
