# Wired and Wireless Network Support

First-line network support is mostly about proving where the problem is: the device, the cable or access point, the local network services, or something further away. This guide complements the Network Troubleshooting Ladder and focuses on wired desks, Wi-Fi, and what a technician maintains and what they hand to the network team.

## 1. What a support technician typically owns

| Usually the technician | Usually the network team |
| --- | --- |
| Patch cables, desk ports and adapters | Switch configuration, VLANs, port security |
| Verifying link, IP and DNS from the device | DHCP scopes, DNS servers, firewalls |
| Wi-Fi settings, profiles and drivers on devices | Access point placement, controllers, SSIDs, RADIUS |
| Labeling, documenting and testing connections | Routing, WAN, internet circuit |
| Collecting evidence and escalating | Changes to the infrastructure |

Follow your organization's boundaries. Do not reconfigure switches or access points without authority.

## 2. Wired troubleshooting

1. **Link.** Check the link light on the PC and the wall or switch port. No light: reseat both ends, then try a known-good patch cable.
2. **Port.** Try another wall port or another known-working device on the same port. This separates a bad port from a bad PC adapter.
3. **Adapter.** Check Device Manager, update the NIC driver, and in the adapter's properties check speed and duplex (Auto Negotiation is normal). Disable power-saving options that turn the adapter off.
4. **Address.** `ipconfig /all`: confirm an address in the expected range, gateway and DNS. `169.254.x.x` means no lease.
5. **Reach.** Ping the gateway, then an outside IP, then resolve a name.
6. **Dock or adapter.** If the PC connects through a dock, test with a direct USB-C or USB Ethernet adapter and update the dock firmware.
7. **Escalate** when the port is dead, the VLAN is wrong, DHCP is not answering, or many users in the same area fail: report the room, port label, switch name if known, time and tests.

Cable basics: Cat5e supports 1 Gbps to 100 m, Cat6 supports 1 Gbps (10 Gbps over shorter runs). A patch cable that is bent sharply, crushed or badly terminated can cause drops. A cable tester finds miswired pairs.

## 3. Wireless troubleshooting

1. Confirm the right SSID, that Wi-Fi is on and Airplane mode is off.
2. Check signal strength and the access point the device is on. Windows: `netsh wlan show interfaces` (SSID, BSSID, signal, radio type, channel). Poor signal is a coverage or distance problem.
3. Compare with another device in the same spot. If both are poor, report a coverage or access point issue. If only one is poor, check the driver and the adapter.
4. Prefer the 5 GHz band where it is available. 2.4 GHz reaches farther but is more crowded.
5. Forget and re-add the network profile if credentials changed. For enterprise (802.1X) networks check the account status, password expiry, certificates and device compliance.
6. Update the Wi-Fi driver and system updates, and turn off adapter power saving.
7. Captive portals: open a plain HTTP page to trigger sign-in.
8. Roaming problems (drops when moving between areas) go to the network team with locations and times.

## 4. Wi-Fi vs wired decision

| Observation | Meaning |
| --- | --- |
| Wired works, Wi-Fi fails on one device | Device Wi-Fi adapter, driver or profile |
| Wi-Fi fails on all devices in an area | Access point or coverage |
| Both fail, other users fine | Device or account problem |
| Both fail, many users | Infrastructure outage: escalate as a major incident |

## 5. Documenting the network environment

Good documentation saves hours. Keep up-to-date: floor plans with port numbering, patch panel and switch port maps, access point locations, VLAN and subnet list, printer IP list, and contacts for the ISP and vendors. Label both ends of every cable. Record changes in the ticket and in the documentation the same day.

## 6. Mobile devices on the network

For tablets and smartphones check: Wi-Fi profile and certificates, the mobile device management enrollment, email and Teams sign-in, and OS updates. Follow the department's mobile device policy for lost devices (report to security immediately).

## 7. Tools

`ipconfig`, `ping`, `tracert`, `pathping`, `nslookup`, `netsh wlan`, `Test-NetConnection`, `arp -a`, a cable tester, a Wi-Fi analyzer app, Wireshark (for authorized capture). Only test systems you are authorized to test.

## 8. Escalation evidence

Location and port or SSID, time, number of affected users, `ipconfig /all`, the first failing step of the ladder, results from another device, and changes made recently.
