# Level 11: Binary Strings

You open `data.txt` and it's a nightmare. 801 lines of random-looking strings. At first glance, it looks like a dictionary attack waiting to happen, but then you realize the pattern: almost every line is a duplicate. 

The goal is to find the one single line that exists alone. If you try to scroll through that list by eye, you're just fighting a losing battle against a screen of noise.

The trick here is to stop looking at the data and start looking at the *frequency*.

First, I used `sort` to group all the duplicates together. Once they're lined up, `uniq -u` becomes the MVP. While standard `uniq` just collapses duplicates, the `-u` flag tells the system: "Only show me the lines that occur exactly once."

```bash
sort data.txt | uniq -u
```
The noise disappears, and you're left with the only line that didn't have a twin.

**Result:** `REDACTED`

**TL;DR:** When you have a massive list of duplicates, don't hunt for the needle—filter out the hay with `sort | uniq -u`.
