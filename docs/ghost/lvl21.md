# Ghost Track - Lvl 21: The Cron Race

So you're in as `ghost20` and your home folder is empty. Classic. When that happens, you gotta stop looking at the files and start looking at how the system actually *behaves*.

If the hint mentions "schedules" or "automated tasks," your brain should immediately go to **Cron**. 

I checked `/etc/cron.d` (which is where admins usually drop custom jobs) and found `ghost-level20`. It's just a one-liner telling you that a script at `/opt/ghost-cron/job.sh` runs every minute as root.

Read the script:
```bash
cat /opt/ghost-cron/job.sh
```

Look at the logic: it reads a secret, dumps it into `/var/tmp/ghost-cron-output`, sleeps for 2 seconds, and then deletes the file. 

Here is the "Aha!" moment: that `sleep 2` is your only window. If you just try to `cat` the file manually, you'll probably just get a "File not found" because you're too slow. You need to "camp" that file.

The trick is to run a simple bash loop. This basically tells the computer: "Check if this file exists. If not, check again. Do this a thousand times a second until you see it, then grab the content and stop."

```bash
while true; do
  if [ -f /var/tmp/ghost-cron-output ]; then
    cat /var/tmp/ghost-cron-output
    break
  fi
done
```

Once the root script drops the file, your loop catches it before the `rm` command even has a chance to run.

**Result:** `REDACTED`

**TL;DR:** Root was leaking a secret to `/var/tmp` for 2 seconds every minute. Just loop it until you catch it.
