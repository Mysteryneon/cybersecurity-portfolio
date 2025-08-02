# Linux - Processes & Services

Enumerating running processes and services helps identify software misconfigurations, vulnerable binaries, or sensitive data in memory. Look for root-owned services, custom daemons, or scheduled jobs.

---

## Commands

```bash
# Show all running processes
ps aux
ps aux --forest
top -n 1

# List listening services and ports
netstat -tulpn
ss -tulpn

# Show scheduled tasks
crontab -l
ls -la /etc/cron*
cat /etc/crontab
cat /etc/anacrontab

# Check systemd services (modern systems)
systemctl list-units --type=service --state=running
```

---

## Example Output

```bash
$ ps aux --forest | head -5
USER       PID  %CPU %MEM COMMAND
root         1  0.0  0.1  /sbin/init
root       720  0.0  1.2  /usr/sbin/apache2 -k start
mysql      853  0.1  5.0  /usr/sbin/mysqld --daemonize
john      1025  0.0  0.1  -bash

$ netstat -tulpn | grep LISTEN
tcp   0   0 0.0.0.0:22      0.0.0.0:*   LISTEN   543/sshd
tcp   0   0 127.0.0.1:3306  0.0.0.0:*   LISTEN   853/mysqld
tcp   0   0 0.0.0.0:8080   0.0.0.0:*   LISTEN   720/apache2

$ crontab -l
* * * * * /usr/local/bin/cleanup.sh
```

---

## Notes

- Look for processes running as `root`, especially ones serving network traffic (e.g. outdated `apache`, `nginx`, `vsftpd`).
- Custom services or misconfigured cron jobs may allow code execution.
- Writable scripts run by cron or systemd timers are excellent privilege escalation vectors.
- Use `systemctl status <service>` to examine any unusual or user-created services.
- Review crontab and timer files for wildcard abuse, unquoted paths, or writable directories.

---

## References

- [HackTricks – Linux Process and Service Enumeration](https://book.hacktricks.xyz/linux-hardening/linux-local-enumeration#processes-and-services)  
- [GTFOBins – Abusable binaries invoked by cron/systemd](https://gtfobins.github.io)
