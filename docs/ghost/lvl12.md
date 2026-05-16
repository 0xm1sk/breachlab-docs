# Level 12: Wrapped Three Deep

This one is a total guessing game. You're handed `data.wrapped`, and the prompt tells you it's nested three layers deep in different compression formats. 

The first instinct is to look at the extension, but `.wrapped` tells us nothing. That's where the `file` command comes in—it's the only way to know what you're actually dealing with without guessing.

`file data.wrapped` reveals it's a **POSIX tar archive**. I unpacked it with `tar -xvf`, and that dropped a file called `core.txt.gz.bz2`. 

Now we're looking at a double extension. I recognized `.bz2` as the outer shell, so I stripped it with `bunzip2`. That left me with `core.txt.gz`. One last layer—a quick `gunzip`—and I finally hit the bottom.

`cat core.txt` and the flag is right there.

**Result:** `REDACTED`

**TL;DR:** Never trust extensions. Use `file` to identify the layer, unpack, and repeat until you hit raw text.
