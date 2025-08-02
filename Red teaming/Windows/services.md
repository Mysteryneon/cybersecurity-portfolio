# Windows - Services, Tasks & DLL Hijacking

Services and scheduled tasks often run with high privileges. Misconfigurations like unquoted paths, writable service binaries, or missing DLLs can lead to privilege escalation.

---

## Commands

```cmd
sc query state= all | find "SERVICE_NAME"
sc qc <ServiceName>
wmic service get Name,DisplayName,PathName,StartMode,StartName
schtasks /query /fo LIST /v
```

---

## Example Output

```cmd
C:\> sc qc BackupService
[SC] QueryServiceConfig SUCCESS
BINARY_PATH_NAME   : C:\Program Files\Backup Tool\backup.exe
START_TYPE         : AUTO_START
SERVICE_START_NAME : LocalSystem

C:\> schtasks /query /fo LIST /v | findstr /I "TaskName Run User"
TaskName: \UpdateTask
Run As User: SYSTEM
```

---

## Notes

- Unquoted service paths with spaces (e.g., `C:\Program Files\...`) can be hijacked if intermediate directories are writable.
- Use `sc qc` and `wmic` to locate these paths and permissions.
- Check if service binaries or directories are writable using `icacls`.
- Scheduled tasks running as SYSTEM or Admin that execute writable scripts or apps can be hijacked.
- DLL hijacking is possible when services attempt to load missing or attacker-controlled DLLs from writable paths (e.g., `C:\` or `%TEMP%`).

---

## References

- [HackTricks – Windows Service Misconfigurations](https://book.hacktricks.xyz/windows-hardening/windows-local-privilege-escalation#services)
- [LOLBAS – DLL Hijacking Techniques](https://lolbas-project.github.io)
- [GTFOBins – Windows Service Hijacking](https://gtfobins.github.io)
