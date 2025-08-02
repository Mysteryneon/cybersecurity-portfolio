# Linux - Network & Listening Ports

Network enumeration helps identify active interfaces, internal IPs, open ports, and potential pivoting opportunities. Look for services listening on high ports, localhost-only bindings, or additional NICs on internal networks.

---

## Commands

```bash
# Network interfaces and IPs
ip a
ip -brief addr
ifconfig

# Routing table
ip route

# Listening ports and services
netstat -tulpn
ss -tulpn

# DNS resolution and hostname
cat /etc/resolv.conf
hostname -I

# Firewall rules (requires root)
iptables -L -n
```

---

## Example Output

```bash
$ ip -brief addr
lo        UNKNOWN        127.0.0.1/8 ::1/128
eth0      UP             10.0.2.15/24
eth1      UP             172.16.5.10/24

$ ss -tulpn | grep LISTEN
tcp    LISTEN  0  128  0.0.0.0:22      0.0.0.0:*  users:(("sshd",pid=543,fd=3))
tcp    LISTEN  0  100  127.0.0.1:3306  0.0.0.0:*  users:(("mysqld",pid=853,fd=12))
tcp    LISTEN  0  128  0.0.0.0:8080   0.0.0.0:*  users:(("apache2",pid=720,fd=4))
```

---

## Notes

- Multiple NICs or subnets may indicate internal infrastructure or pivoting opportunities.
- Services bound to 127.0.0.1 are not externally exposed but are accessible locally.
- Look for uncommon high ports (>1024), which may host custom applications.
- If `iptables` rules are permissive or missing, reverse shells may be easily established.
- DNS settings (`/etc/resolv.conf`) may reveal internal domain names or DNS servers.

---

## References

- [HackTricks – Linux Networking Enumeration](https://book.hacktricks.xyz/linux-hardening/linux-local-enumeration#network)  
- [GTFOBins – Networking Utilities](https://gtfobins.github.io)
