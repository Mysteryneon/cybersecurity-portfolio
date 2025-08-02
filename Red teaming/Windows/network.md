# Windows - Network, Shares & Ports

Enumerating network interfaces, open ports, and shared folders can help identify reachable services, pivot opportunities, or accessible file shares for data access or privilege escalation.

---

## Commands

```cmd
ipconfig /all
route print
netstat -ano
tasklist /fi "PID eq <pid>"
net share
net use
net session
```

---

## Example Output

```cmd
C:\> ipconfig /all
Ethernet adapter Ethernet0:
   IPv4 Address. . . . . . . . . . . : 10.10.20.15

C:\> netstat -ano | find "LISTEN"
TCP    0.0.0.0:3389    LISTENING    1234
TCP    127.0.0.1:8000  LISTENING    1122

C:\> net share
Share name   Resource                        Remark
----------   --------                        ------
C$           C:\                            Default share
SharedDocs   C:\Users\Public\Documents    User files
```

---

## Notes

- `ipconfig` reveals IPs, DNS servers, and DHCP info — useful for identifying internal subnets.
- `netstat` + `tasklist` help map listening ports to applications and services.
- Services on 127.0.0.1 may not be exposed externally but can be accessed locally for exploitation.
- `net share` lists shared folders that may be accessible or misconfigured.
- `net session` (Admin-only) shows active SMB connections to/from the host.

---

## References

- [HackTricks – Windows Networking Enumeration](https://book.hacktricks.xyz/windows-hardening/windows-local-privilege-escalation#network)
- [LOLBAS – Network Utilities](https://lolbas-project.github.io)
