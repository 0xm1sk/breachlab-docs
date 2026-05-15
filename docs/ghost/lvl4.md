# Ghost Track - Lvl 4: Access Denied

This level teaches you about Linux permissions and groups. Not every user can read every file, but you might belong to a **group** that has the necessary access.

Check the `map.txt` file in your home directory. It tells you that `/var/intel/ops/` is restricted to the `analysts` group.

Try to list the files in that directory:
```bash
ls -la /var/intel/ops/
```
You'll see files like `access_codes.dat`. If you try to `cat` it and get "Permission Denied," you need to check who you are.

Run the `id` command:
```bash
id
```
Look at the `groups=` section. You'll see that your user (`ghost3`) is part of the `analysts` group. Because the file `/var/intel/ops/access_codes.dat` has read permissions for the group `analysts`, you can actually read it despite not being the owner.

```bash
cat /var/intel/ops/access_codes.dat
```

**Result:** `REDACTED`

**TL;DR:** Permissions aren't just User/Group/Other. Always check your own group membership with `id` to see if you have a "backdoor" into a restricted directory.
