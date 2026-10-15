# Knowledge Base and Documentation

Good documentation turns one solved ticket into many avoided tickets. This page explains how to write and maintain a Service Desk Wiki or knowledge base, and includes nine sample articles you can reuse or adapt. The same articles are in the `service-desk-knowledge-base` repository.

## 1. What makes a good article

- **Findable:** the title uses the words a user would type.
- **Scoped:** it says what it applies to (system, version, audience).
- **Actionable:** short numbered steps that start with a verb.
- **Verifiable:** it says how to know the fix worked.
- **Safe:** it warns before data loss or steps that need approval, and never includes passwords.
- **Maintained:** it has a review date, and wrong articles are fixed or retired quickly.

## 2. Article template

```text
Title: KB-XXXX: <problem in the user's words>
Applies to / Audience / Last reviewed
Symptom
Cause
Resolution (numbered steps)
How to verify
If this does not work (escalation and evidence)
```

## 3. When to write an article

1. The same issue has appeared three or more times.
2. A fix took research that the next person should not repeat.
3. A procedure is rarely used but risky to get wrong (password reset, device wipe).
4. A change is rolled out and users will ask about it.

## 4. Keeping the wiki healthy

- Review articles on a schedule (every six to twelve months) and after major changes.
- Link articles from ticket resolutions so usage is visible.
- Ask users or colleagues to test new articles.
- Use consistent categories, templates and naming.
- Keep a short index page by category, and a "recently updated" list.

## 5. Systems documentation

Beyond articles, keep current: an equipment inventory, network and port maps, printer list with addresses, standard image contents, software and license list, vendor and support contacts, and room sheets for conference rooms. Record changes in the ticket and update the document the same day.

## 6. Sample articles

### KB-0001: Outlook search returns no or incomplete results

| Field | Value |
| --- | --- |
| Applies to | Outlook for Windows (Microsoft 365 Apps), Windows 10/11 |
| Audience | End users and service desk |
| Last reviewed | 2026-10-15 |

**Symptom**
Searching in Outlook shows "No results" or misses recent messages, while the messages are visible in Outlook on the web.

**Cause**
The local Windows Search index is incomplete or corrupted, or Outlook is set to keep only a short window of mail offline.

**Resolution**

1. Confirm the messages appear in Outlook on the web. If they do not, stop and escalate: this is not an index problem.
2. In Outlook choose **File > Options > Search > Indexing Options**.
3. Choose **Advanced > Rebuild**. Indexing can take minutes to hours on large mailboxes.
4. Confirm the **Windows Search** service is running (`services.msc`).
5. Under **File > Account Settings > Account Settings > Change**, raise **Mail to keep offline** if needed.
6. Restart Outlook and test again after indexing finishes.

**How to verify**
A search for a known recent message returns it, and Indexing Status shows 0 items remaining.

**If this does not work**
Create a new Outlook profile and test. Escalate to Tier 2 with the Outlook build, the profile test result and the indexing status.


### KB-0002: Network printer shows Offline or will not print

| Field | Value |
| --- | --- |
| Applies to | Windows 10/11 with network printers |
| Audience | Service desk and deskside support |
| Last reviewed | 2026-10-15 |

**Symptom**
A printer shows **Offline**, or jobs stay in the queue and nothing prints.

**Cause**
Common causes: the printer is off or has moved to a new IP address, the port or driver on the PC is wrong, the queue is stuck, or the network path is blocked.

**Resolution**

1. Check the printer panel for errors (paper, jam, toner) and print a configuration page from the printer. Note its IP address.
2. From the PC, `ping <printer IP>`. No reply means a network or power problem: check the cable and port, and ask whether others can print.
3. In **Printer properties > Ports**, confirm the port points to the correct IP address. Printers should have a static or reserved address.
4. Open the print queue and cancel stuck jobs. If they will not clear, restart the Print Spooler service (with approval for the machine).
5. Confirm the approved driver is installed. Remove and re-add the printer if the port or driver is wrong.
6. Print a test page, then ask the user to print the real document.

**How to verify**
The printer shows Ready and the user's document prints.

**If this does not work**
Escalate with the printer name, IP, test results (ping, test page), number of affected users and the steps tried. Secure-print or print-server problems go to the print server owner.


### KB-0003: Wi-Fi shows connected but there is no internet

| Field | Value |
| --- | --- |
| Applies to | Windows 10/11 and macOS on wireless networks |
| Audience | End users and service desk |
| Last reviewed | 2026-10-15 |

**Symptom**
The Wi-Fi icon shows connected, but web pages and applications do not load.

**Cause**
The device may have no valid DHCP lease, a wrong DNS server, a captive portal that needs sign-in, or a weak signal.

**Resolution**

1. Check whether other devices on the same network work. If none do, report a network outage with the location and time.
2. Turn Wi-Fi off and on, then reconnect. Move closer to the access point.
3. Windows: run `ipconfig /all`. An address starting `169.254` means no lease: run `ipconfig /release` then `ipconfig /renew`.
4. Run `ping 1.1.1.1`. If it works but websites fail, DNS is the problem: run `nslookup example.com` and check the DNS servers, then `ipconfig /flushdns`.
5. Open a browser to a plain HTTP page to trigger any captive portal sign-in.
6. Forget the network and join it again with the correct credentials.
7. Update the Wi-Fi driver, and test with a wired connection to separate the device from the wireless network.

**How to verify**
Websites and the user's applications load.

**If this does not work**
Escalate to the network team with the SSID, location, `ipconfig /all` output, ping and nslookup results, and which other devices are affected.


### KB-0004: Microsoft Teams microphone or camera not working

| Field | Value |
| --- | --- |
| Applies to | Teams on Windows 10/11 and macOS |
| Audience | End users, service desk, conference-room support |
| Last reviewed | 2026-10-15 |

**Symptom**
In a Teams call others cannot hear you or see you, or Teams says no microphone or camera was found.

**Cause**
Wrong device selected in Teams, operating system privacy permission blocked, another app using the device, or a loose cable or dead battery.

**Resolution**

1. Check the physical device: cable seated, headset powered and not muted, camera shutter open.
2. In Teams choose **Settings > Devices** and select the correct microphone, speaker and camera. Use **Make a test call**.
3. Windows: **Settings > Privacy & security > Microphone / Camera** and allow desktop apps. macOS: **System Settings > Privacy & Security > Microphone / Camera** and enable Teams, then restart Teams.
4. Close other apps that may hold the device (Zoom, browser tabs, recording tools).
5. Try another USB port, and test the device on another computer to separate the device from the PC.
6. Restart Teams, then the computer. Update the device driver or firmware.

**How to verify**
The test call shows the microphone level moving and the camera preview.

**If this does not work**
Escalate with the device model, the OS version, the Teams version and the results of testing on another computer.


### KB-0005: Password reset and account unlock (service desk procedure)

| Field | Value |
| --- | --- |
| Applies to | Active Directory and Microsoft 365 accounts |
| Audience | Service desk |
| Last reviewed | 2026-10-15 |

**Symptom**
A user cannot sign in because they forgot their password, it expired, or the account is locked.

**Cause**
Forgotten or expired password, repeated failed sign-ins, or an old saved password on another device or app.

**Resolution**

1. **Verify identity first** using the department's procedure, for example a call-back to the number in the directory, or manager approval. Never rely on an email request alone.
2. Check the account is enabled and not locked for another reason. If it is disabled, stop and find out why.
3. Reset the password with a unique temporary value and tick **User must change password at next logon**. Unlock the account if needed.
4. Give the temporary password only through the approved channel. Never write it in the ticket.
5. Tell the user to update saved credentials on other devices: phone email, Wi-Fi, mapped drives, VPN and Teams.
6. If lockouts repeat, look for the source (old phone, mapped drive, scheduled task) and escalate to security if the source is unknown.
7. Record who was verified, how, and the time. Do not record the password.

**How to verify**
The user signs in and is prompted to set a new password.

**If this does not work**
Escalate to the identity team with the username, the lockout source (if known) and the verification record.


### KB-0006: OneDrive is not syncing

| Field | Value |
| --- | --- |
| Applies to | OneDrive sync client on Windows 10/11 |
| Audience | End users and service desk |
| Last reviewed | 2026-10-15 |

**Symptom**
The OneDrive icon shows a red X, a paused or sync-pending state, or files do not appear on other devices.

**Cause**
Account signed out, storage full, file open in an app, invalid characters or a very long path, or a stale local state.

**Resolution**

1. Click the OneDrive cloud icon and read the message. Open **View sync problems** for the failing files.
2. Confirm sign-in with the work account and that storage is not full.
3. Close apps that have the file open. Rename files with invalid characters (`" * : < > ? / \ |`) or shorten long paths.
4. Pause and resume sync.
5. If it still fails, reset OneDrive: press Windows+R and run `%localappdata%\Microsoft\OneDrive\onedrive.exe /reset`. Files are not deleted, but a full re-sync follows.
6. Compare with the OneDrive web site to confirm the files are safe in the cloud.

**How to verify**
The icon shows a green check, and a test file appears on the web and on another device.

**If this does not work**
Escalate with the OneDrive version, the exact sync error, and the file path.


### KB-0007: Conference room display shows no picture

| Field | Value |
| --- | --- |
| Applies to | Conference rooms with a display or projector and HDMI, USB-C or wireless casting |
| Audience | Service desk, deskside and AV support |
| Last reviewed | 2026-10-15 |

**Symptom**
A laptop is connected in a conference room, but the display shows "No signal" or a blank screen.

**Cause**
Wrong input selected on the display, loose or failed cable or adapter, laptop not sending output, or an unsupported resolution.

**Resolution**

1. Confirm the display is on and select the correct input with the remote or the room panel.
2. Reseat both ends of the cable. Try a second cable or adapter.
3. On the laptop press **Windows+P** and choose **Duplicate** or **Extend**. On macOS use **System Settings > Displays > Detect displays**.
4. Set a common resolution (1920 x 1080) and 60 Hz in display settings.
5. Try another laptop to find out whether the fault is in the room or the laptop.
6. For wireless casting, confirm both devices are on the right network, then restart casting.
7. For room systems with a control panel or matrix switch, restart only as the room owner procedure allows.

**How to verify**
The laptop screen appears on the display, and audio plays through the room speakers if required.

**If this does not work**
Open a ticket for the AV vendor or facilities with the room number, equipment model, tests done and the time of the next meeting.


### KB-0008: New user device setup checklist

| Field | Value |
| --- | --- |
| Applies to | New employees receiving a laptop or desktop |
| Audience | Service desk and deskside support |
| Last reviewed | 2026-10-15 |

**Symptom**
A new employee needs a working computer on day one.

**Cause**
Not applicable. This is a planned request (install).

**Resolution**

1. Confirm the approved request: name, role, manager, start date, location.
2. Assign an asset, record the tag and serial number in the inventory, and apply the approved image.
3. Confirm updates, disk encryption and management agents, and install the standard applications.
4. Create the account and mailbox, assign group memberships and licenses as approved.
5. On day one verify identity, sign in with a temporary password and force a change, and set up MFA.
6. Test email, Teams, OneDrive, VPN, printers, shared drives and department applications.
7. Walk the user through how to get help, and have them accept the equipment.
8. Update the inventory with the assigned user, date and location, then close the ticket.

**How to verify**
The user completes sign-in and opens their key applications. The inventory shows the correct owner.

**If this does not work**
Escalate missing access to the application owner with the approval reference, and missing hardware to purchasing or the vendor.


### KB-0009: Computer is slow

| Field | Value |
| --- | --- |
| Applies to | Windows 10/11 desktops and laptops |
| Audience | Service desk and deskside support |
| Last reviewed | 2026-10-15 |

**Symptom**
The computer is slow to start, open applications or respond.

**Cause**
Common causes: low free disk space, too many startup programs, pending updates, a runaway process, low memory or a failing drive.

**Resolution**

1. Restart the computer (not a shutdown, because Fast Startup keeps the session).
2. Open Task Manager and sort by CPU, Memory and Disk to find a process using most of the resources.
3. Check free disk space. Under about 10% free causes trouble. Run Storage Sense. Do not delete user files without permission.
4. Review **Startup apps** in Task Manager and disable unneeded entries with the user's agreement.
5. Check for pending Windows updates or an active antivirus scan.
6. Run `Get-PhysicalDisk | Select FriendlyName,HealthStatus` to check drive health. If the drive is unhealthy, back up data and request a replacement.
7. Run `sfc /scannow` and `DISM /Online /Cleanup-Image /RestoreHealth` if system files may be damaged.

**How to verify**
Startup time and application launch time return to normal, and Task Manager shows no sustained 100% usage.

**If this does not work**
Escalate for a hardware check or reimage, with Task Manager screenshots, disk space and health output, and the time the slowness started.

