# Level 15: TLS, Not Plaintext

Same setup as the last level, but this time the service has upgraded. It's now speaking TLS. If you try to use plain `nc`, it'll just hang or error out because the server is expecting an encrypted handshake that `nc` doesn't know how to do.

You need a tool that understands SSL/TLS. `openssl s_client` is the way to go. 

A pro tip here: use the `-quiet` flag. Without it, OpenSSL dumps a mountain of certificate data onto your screen before it actually lets you talk to the service. With it, the output is clean and direct.

```bash
openssl s_client -quiet -connect 127.0.0.1:30001
# Input: REDACTED
```
Once the encrypted tunnel is established, the service greets you, you send the password, and you get the next one.

**Result:** `REDACTED`

**TL;DR:** `nc` is for plaintext; `openssl s_client` is for encrypted (TLS) services.
