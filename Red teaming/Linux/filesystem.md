# Linux - Filesystem, Permissions & SUID

Reviewing the file system can expose sensitive files, weak permissions, misconfigurations, and binaries with privilege escalation potential such as SUID or writable scripts.

---

## Commands

```bash
# Find world-writable directories (excluding /proc)
find / -type d -perm -0002 -print 2>/dev/null | grep -v /proc

# Find world-writable files
find / -type f -perm -0002 -print 2>/dev/null | grep -v /proc

# Search for SUID binaries
find / -perm -4000 -type f 2>/dev/null

# Search for SGID binaries
find / -perm -2000 -type f 2>/dev/null

# Look for potential password or key leaks
grep -Ri "password" /etc/ 2>/dev/null
grep -Ri "password" /var/www/ 2>/dev/null

# Check capabilities (alternative to SUID)
getcap -r / 2>/dev/null
```

---

## Example Output

```bash
$ find / -perm -4000 -type f 2>/dev/null | grep -v /snap
/usr/bin/passwd
/usr/bin/sudo
/usr/bin/nmap
```

---

## Notes

- World-writable files/directories can be abused to place or modify executables/scripts.
- Uncommon SUID binaries (like `nmap`, `vim`, `find`) may allow shell escapes. Check them on [GTFOBins](https://gtfobins.github.io).
- Look for sensitive config files (e.g., `config.php`, `.env`, `.bak`) containing hardcoded credentials or keys.
- Capabilities (`getcap`) can grant binaries special permissions (e.g., `cap_setuid+ep`) without SUID bit. These can be exploited similarly.
- Review home directories, backups, and old dev folders for leftover data or SSH keys.

---

## References

- [HackTricks – SUID Binaries & Writable Paths](https://book.hacktricks.xyz/linux-hardening/privilege-escalation#suid)  
- [GTFOBins – SUID Exploitable Binaries](https://gtfobins.github.io)  
- [HackTricks – Linux File Permissions](https://book.hacktricks.xyz/linux-hardening/file-permissions)
