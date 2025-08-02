# Windows - Users, Groups & Privileges

Enumerating users and group memberships helps identify accounts with high privileges, impersonation rights, or misconfigurations that can be leveraged for privilege escalation.

---

## Commands

```cmd
whoami /all
net user
net user <username>
net localgroup administrators
net localgroup
net localgroup "Remote Desktop Users"
```

---

## Example Output

```cmd
C:\> whoami /all
User Name     : WINLAB\lowpriv
Groups        : Everyone
                Users
                Remote Desktop Users
Privileges    : SeImpersonatePrivilege
                SeChangeNotifyPrivilege

C:\> net user
User accounts for \TARGET
---------------------------------------------------
Administrator  Guest  john  svc_backups  lowpriv

C:\> net localgroup administrators
Administrator
WINLAB\Domain Admins
```

---

## Notes

- `whoami /all` lists privileges (e.g., `SeImpersonatePrivilege` is exploitable via Potato attacks).
- `net user` and `net localgroup` show local users and groups.
- Look for service accounts (`svc_*`) and domain groups like `Domain Admins`.
- If the current user is part of `Administrators`, UAC may still limit privilege — use token elevation techniques.
- Check if user has SeAssignPrimaryToken, SeBackup, or other special privileges for lateral or vertical movement.

---

## References

- [HackTricks – Token & Privilege Abuse](https://book.hacktricks.xyz/windows-hardening/windows-local-privilege-escalation#privileges)
- [LOLBAS – Native Windows Tools for Abuse](https://lolbas-project.github.io)
