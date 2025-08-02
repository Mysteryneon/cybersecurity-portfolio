# Windows - System Info & Patch Level

Initial system-level enumeration helps determine the OS version, installed patches, architecture, and domain membership — all of which are essential for selecting the right exploitation methods.

---

## Commands

```cmd
systeminfo
wmic os get Caption,OSArchitecture,Version
wmic qfe get HotFixID,InstalledOn
echo %PROCESSOR_ARCHITECTURE%
hostname
whoami
whoami /all
```

---

## Example Output

```cmd
C:\> systeminfo | findstr /B /C:"OS Name" /C:"OS Version"
OS Name:                   Microsoft Windows Server 2016 Standard
OS Version:                10.0.14393 N/A Build 14393

C:\> wmic qfe get HotFixID,InstalledOn
HotFixID      InstalledOn
KB5003637     6/10/2021
KB5001342     5/5/2021

C:\> whoami
WINLAB\lowpriv
```

---

## Notes

- `systeminfo` gives a quick view of OS build, patch level, domain membership, and installed RAM.
- `wmic qfe` lists hotfixes; missing patches may correlate with known LPEs (check with Watson or WES).
- `%PROCESSOR_ARCHITECTURE%` reveals 32 vs 64-bit — important for payload compatibility.
- `whoami /all` helps detect high-privilege tokens or impersonation rights (e.g., SeImpersonatePrivilege).

---

## References

- [HackTricks – Windows Local Enumeration](https://book.hacktricks.xyz/windows-hardening/windows-local-privilege-escalation)  
- [Watson / WES-NG – Missing Patch Analysis](https://github.com/rasta-mouse/Watson)
