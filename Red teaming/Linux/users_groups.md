# Linux - Users & Groups

Understanding user and group configurations can uncover privilege escalation paths, misconfigurations, and leftover accounts from previous users or services.

---

## Commands

```bash
# Show current user and their groups
whoami
id
groups

# List all system users
cut -d: -f1 /etc/passwd

# Show detailed user and password-related info
cat /etc/passwd
cat /etc/shadow    # Requires elevated permissions

# Show group memberships
getent group
cat /etc/group

# Check sudo privileges
sudo -l

# Check password aging and policies
chage -l <username>
cat /etc/login.defs
```

---

## Example Output

```bash
$ whoami
john

$ id
uid=1001(john) gid=1001(john) groups=1001(john),27(sudo)

$ cut -d: -f1 /etc/passwd | head -5
root
daemon
bin
sys
john

$ sudo -l
User john may run the following commands on this host:
    (ALL : ALL) ALL
```

---

## Notes

- The `/etc/passwd` file contains all local users. Look for users that aren't system accounts.
- `/etc/shadow` contains password hashes. Access requires root. If readable, hashes may be cracked.
- `id` and `groups` help reveal if your current user has elevated rights (e.g. in `sudo`, `docker`, `adm`, or `lxd` groups).
- `sudo -l` lists allowed commands. Even limited commands may be abusable. Check each one against [GTFOBins](https://gtfobins.github.io).
- Group `docker` or `lxd` may allow container-based privilege escalation.
- Weak password policies (`/etc/login.defs`) might allow brute-force or reused credentials.

---

## References

- [HackTricks – Linux User Enumeration](https://book.hacktricks.xyz/linux-hardening/linux-local-enumeration#user-enumeration)  
- [GTFOBins – Exploitable sudo and group binaries](https://gtfobins.github.io)
