Problem

Compute host was not reachable from other devices on the network.

Environment
Fedora 43 (compute host)
Home network with ISP router + previously used Google Nest mesh

Subnets involved:
192.168.86.0/24 (old – Nest)
192.168.0.0/24 (current – ISP router)

Symptoms
Compute host could ping gateway
Laptop could ping gateway
Compute host could ping laptop
Laptop could not ping compute host
Expected IP: 192.168.86.26
Host not visible in nmap scan

Investigation
Sent broadcast ping → only expected devices responded
Ran nmap scan → device count correct, but compute host IP missing
Checked router → ARP/IP table empty (unexpected, noted for later)
Verified /etc/hosts → hostname mapping correct

Root Cause
Network subnet mismatch after removing Google Nest mesh:

Compute host was configured for 192.168.86.0/24
Current network was 192.168.0.0/24
Result: host had connectivity in one direction but was not properly addressable on the network

Fix
Reserved correct IP for compute host on router (192.168.0.x)
Updated /etc/hosts with new IP
Restarted network interface
Verified connectivity from laptop → success

Lessons Learned
Always verify subnet when changing network infrastructure
Don’t trust expected IPs — confirm actual network config first
Partial connectivity (one-way ping) can indicate misconfigured addressing
Router not showing ARP table = separate issue worth investigating
