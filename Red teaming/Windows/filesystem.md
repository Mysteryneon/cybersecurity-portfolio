# Windows - Filesystem & Registry Misconfigurations

File and registry misconfigurations such as writable executables, exposed passwords, or autorun entries can be leveraged to escalate privileges or gain persistence.

---

## Commands

```cmd
icacls "C:\Program Files\"
icacls "C:\path\to\binary.exe"

reg query HKLM\Software\Microsoft\Windows\CurrentVersion\Run
reg query HKCU\Software\Microsoft\Windows\CurrentVersion\Run

findstr /si password *.txt *.xml *.ini *.config
dir /s /b *unattend.xml
```

---

## Example Output

```cmd
C:\> icacls "C:\Program Files\App\"
BUILTIN\Users:(I)(M)
BUILTIN\Administrators:(I)(F)

C:\> reg query HKLM\...\Run
(Default)    REG_SZ    C:\scripts\startup.bat

C:\> findstr /si password *.config
web.config:<add key="db_password" value="Secret123!" />
```

---

## Notes

- Writable executables or folders in `Program Files` or `System32` can be replaced and hijacked if run by a privileged service.
- Registry autorun entries that point to user-writable paths can be used for escalation or persistence.
- Files like `unattend.xml`, `sysprep.xml`, and `.config` files often contain plain-text credentials.
- Use `icacls` or Sysinternals `AccessChk` to systematically enumerate ACL weaknesses.

---

## References

- [HackTricks – Windows Filesystem & Registry Checks](https://book.hacktricks.xyz/windows-hardening/windows-local-privilege-escalation#filesystem-and-registry)
- [LOLBAS – Filesystem Abuse](https://lolbas-project.github.io)
