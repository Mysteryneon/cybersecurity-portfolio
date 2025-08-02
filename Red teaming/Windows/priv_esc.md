# Windows - Privilege Escalation Vectors

Correlate enumeration findings to known privilege escalation techniques including vulnerable services, token impersonation, AlwaysInstallElevated, unquoted paths, and missing patches.

---

## Common Paths & Validation Commands

```cmd
whoami /priv
sc qc <ServiceName>
reg query HKLM\...\AlwaysInstallElevated
accesschk.exe -uwcqv "Users" *
icacls "C:\Program Files\App"
```

---

## Example Scenarios

- **AlwaysInstallElevated**
  ```cmd
  C:\> reg query HKCU\...\AlwaysInstallElevated
  0x1
  C:\> reg query HKLM\...\AlwaysInstallElevated
  0x1
  ```
  → Create a malicious MSI and install as SYSTEM.

- **Service Binary Writable**
  ```cmd
  C:\> icacls "C:\Program Files\App\service.exe"
  BUILTIN\Users:(M)
  ```
  → Replace binary and restart service to gain SYSTEM.

- **Unquoted Service Path**
  ```cmd
  BINARY_PATH_NAME   : C:\Program Files\Vuln App\service.exe
  ```
  → Place `C:\Program.exe` to hijack.

- **Token Impersonation**
  ```cmd
  SeImpersonatePrivilege = Enabled
  ```
  → Use JuicyPotato/RoguePotato with a suitable COM server.

---

## Notes

- Use tools like WinPEAS, PowerUp, or Seatbelt for automated checks.
- Leverage LOLBAS to identify native binaries exploitable for escalation.
- Kernel exploits may be considered as a last resort after patch checks.
- Ensure both HKCU and HKLM AlwaysInstallElevated are set to 1 before exploiting.

---

## References

- [HackTricks – Windows Privilege Escalation](https://book.hacktricks.xyz/windows-hardening/windows-local-privilege-escalation)
- [LOLBAS – Escalation Techniques](https://lolbas-project.github.io)
- [WinPEAS – Local PE Tool](https://github.com/carlospolop/PEASS-ng)
