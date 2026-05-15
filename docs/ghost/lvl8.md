# Ghost Track - Lvl 8: Lost in Translation

This level introduces "multi-layer" encoding. The secret isn't just Base64; it's wrapped in another format first.

You'll find a file called `transmission.dat`. If you `cat` it, you'll see a hex dump (columns of numbers and letters like `5244 4e6a`). 

To solve this, you have to "peel the onion":
1. **Hex to Binary**: Use `xxd -r` to convert the hex text back into actual binary data.
2. **Binary to Text**: The resulting data is still Base64 encoded, so pipe it into `base64 -d`.

The one-liner looks like this:
```bash
cat transmission.dat | xxd -r | base64 -d
```

**Result:** `REDACTED`

**TL;DR:** If a file looks like a hex dump, use `xxd -r` to reverse it. If the result is still gibberish, try `base64 -d`. Always keep piping until the data becomes human-readable.
