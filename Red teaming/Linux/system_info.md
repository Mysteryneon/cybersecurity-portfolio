# 🐧 Linux - System Info & OS Details

Collecting system information is the first step to understanding your environment. This includes OS version, kernel, environment variables, hostname, and more — all of which help inform your attack strategy.

---

## 🔧 Commands

```bash
# Kernel and OS version
uname -a
lsb_release -a
cat /etc/os-release

# Hostname and environment
hostname
whoami
id
printenv
```

---

## 🧪 Example Output

```bash
$ uname -a
Linux target 5.4.0-77-generic #86~18.04.1-Ubuntu SMP x86_64 GNU/Linux

$ lsb_release -a
Distributor ID: Ubuntu
Description:    Ubuntu 18.04.5 LTS
Release:        18.04
Codename:       bionic

$ whoami
john

$ id
uid=1001(john) gid=1001(john) groups=1001(john),27(sudo)

$ printenv | grep -i user
USER=john
```

---

## 📌 Notes

- `uname -a`: Reveals the kernel version — useful for identifying kernel exploits.
- `lsb_release` or `/etc/os-release`: OS and distribution info (helps with exploit compatibility).
- `whoami`, `id`: Confirms user context and group memberships.
- `printenv`: Environment variables may contain credentials or paths (e.g., AWS keys, proxies, etc.).
- Always check if you're root already (`whoami`) before escalating.

---

## 🔗 References

- [HackTricks – Linux Privilege Escalation Checklist](https://book.hacktricks.xyz/linux-hardening/privilege-escalation-checklist)
- [GTFOBins – Linux Binaries for PE](https://gtfobins.github.io)
