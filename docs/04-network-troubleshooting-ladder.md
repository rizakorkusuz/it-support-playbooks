# Network Troubleshooting Ladder

Work from the bottom of the stack upward and stop at the first layer that fails. Each rung has a quick test and a decision: if it passes, move up. If it fails, fix that layer before testing the next. The commands are for Windows (macOS and Linux equivalents are in the table at the end).

## The ladder

| # | Layer | Question | Test | Healthy result |
| --- | --- | --- | --- | --- |
| 1 | Physical | Is there a link? | Cable seated, link lights, Wi-Fi icon, try another port or cable | Link light on, adapter shows Connected |
| 2 | Adapter | Is the adapter up with an address? | `ipconfig /all` | IPv4 address in the expected range, subnet mask, default gateway, DNS servers |
| 3 | Local IP | Is the stack working? | `ping 127.0.0.1`, then ping own IP | Replies |
| 4 | Gateway | Can I reach the router? | `ping <default gateway>` | Replies, low latency |
| 5 | Remote IP | Can I reach the internet by IP? | `ping 1.1.1.1` or `ping 8.8.8.8` | Replies |
| 6 | DNS | Can names be resolved? | `nslookup example.com` | Returns an address |
| 7 | Transport | Is the port open? | `Test-NetConnection example.com -Port 443` | `TcpTestSucceeded : True` |
| 8 | Application | Does the app work? | Browser, app logs, `curl -I https://example.com` | Expected HTTP status |

## Reading the results

**Step 2: wrong address**

- `169.254.x.x` (APIPA): the device did not get a DHCP lease. Check the cable or Wi-Fi, VLAN, DHCP scope exhaustion or the DHCP server. Try `ipconfig /release` then `ipconfig /renew`.
- Address outside the expected range or a different subnet: a rogue DHCP server, wrong VLAN or a static address left on the adapter.
- No default gateway: gateway is not offered or the adapter is set manually.
- Duplicate address warnings point to a static IP conflict.

**Step 4: gateway unreachable**
Wrong VLAN or SSID, bad cable or port, switch port disabled, Wi-Fi signal or captive portal. Some routers block ICMP, so also test by opening the router page.

**Step 5 passes but step 6 fails**
The connection works but DNS does not. Compare `nslookup example.com` against `nslookup example.com 1.1.1.1`. If the second works, the configured DNS server is the problem. Check DNS servers on the adapter, DHCP options, and VPN DNS settings. Flush the cache with `ipconfig /flushdns`.

**Step 6 passes, step 7 fails**
The name resolves but the port is not reachable: firewall, proxy, the service is down, or the wrong address (stale DNS). Test from another device to see whether it is only this client.

**Intermittent drops**
Run a continuous ping (`ping -t <target>`), look for loss or high latency, and test with a wired connection to separate Wi-Fi issues. Check channel, signal, roaming, duplex mismatch, bad cable, and VPN reconnects. Use `pathping <target>` for per-hop loss.

## Name resolution specifics

- Resolve an internal name: `nslookup server01 <internal-dns>`. A "Non-existent domain" answer means the record is missing. A timeout means the server is unreachable.
- Check the DNS suffix search list when short names fail but FQDNs work.
- A stale hosts file entry can override DNS: `C:\Windows\System32\drivers\etc\hosts`.
- Use `Resolve-DnsName example.com -Type A -Server 1.1.1.1` for richer output.

## Wi-Fi checklist

1. Confirm the correct SSID, that Airplane mode is off, and the radio switch is on.
2. `netsh wlan show interfaces` shows SSID, signal, radio type, channel and authentication.
3. Forget and re-add the network if the credentials changed (`netsh wlan delete profile name="<ssid>"`).
4. Update the driver, and check the power-saving setting on the adapter.
5. For enterprise Wi-Fi (802.1X), check certificates, the password and device compliance, and whether the user account is locked.

## VPN checklist

1. Confirm basic internet access works before VPN.
2. Check the client version, credentials and MFA prompt.
3. After connecting, check the assigned address, routes (`route print`) and DNS servers.
4. Confirm split-tunnel behavior and which resources are expected over VPN.
5. Common failures: blocked by a hotel or public Wi-Fi, expired password, time skew, outdated client, ISP or firewall blocking the VPN port.

## Evidence for escalation

Include: output of `ipconfig /all`, the failing step, `tracert <target>` or `pathping`, times and scope (which users, which locations), whether the issue exists on wired and wireless, and what changed recently.

## Command equivalents

| Task | Windows | macOS | Linux |
| --- | --- | --- | --- |
| IP configuration | `ipconfig /all` | `ifconfig`, `networksetup -getinfo Wi-Fi` | `ip addr`, `nmcli` |
| Renew DHCP | `ipconfig /release` and `/renew` | System Settings > Network > Renew DHCP Lease | `sudo dhclient -r`, `sudo dhclient` |
| DNS lookup | `nslookup`, `Resolve-DnsName` | `dig`, `nslookup` | `dig`, `resolvectl query` |
| Flush DNS | `ipconfig /flushdns` | `sudo dscacheutil -flushcache; sudo killall -HUP mDNSResponder` | `resolvectl flush-caches` |
| Trace route | `tracert` | `traceroute` | `traceroute`, `mtr` |
| Port test | `Test-NetConnection -Port` | `nc -vz host port` | `nc -vz host port` |
| ARP table | `arp -a` | `arp -a` | `ip neigh` |
| Routes | `route print` | `netstat -rn` | `ip route` |

## Safety

Only test systems you are authorized to troubleshoot. Do not run port scans on networks you do not manage without permission.
