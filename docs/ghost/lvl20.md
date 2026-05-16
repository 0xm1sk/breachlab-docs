# Level 20: Your First Script

A service is asking for a password and a 4-digit PIN. There are 10,000 possible PINs (0000 to 9999). 

If you try to type these by hand, you're not an operator; you're a typewriter. This is the perfect moment to stop manual work and start automating. 

The goal is to create a loop that tries every single PIN until the service stops telling us "Wrong." I used `seq -w` to generate the numbers (the `-w` is key because it keeps the leading zeros, so 1 becomes 0001) and piped them into `nc`.

```bash
for pin in $(seq -w 0000 9999); do
    echo "REDACTED $pin" | nc -w 1 localhost 30002 | grep -v Wrong && break
done
```
The `grep -v Wrong` part tells the loop: "Keep going as long as the response contains the word 'Wrong'. The moment it doesn't, stop."

The loop breaks, and the correct password is revealed.

**Result:** `REDACTED`

**TL;DR:** When the possibilities are finite but too many for a human, write a Bash loop with `seq` and `nc` to brute-force the answer.
