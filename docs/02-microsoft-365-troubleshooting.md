# Microsoft 365, Outlook, Teams and OneDrive Troubleshooting

Most Microsoft 365 tickets fall into five buckets: sign-in, licensing, Outlook, Teams and OneDrive sync. Work from the account outward: identity, license, service health, client, then network.

## 1. Always start here

1. Check **Microsoft 365 Service health** in the admin center (if you have access) or the public status page. A tenant-wide incident means communicate and wait, not troubleshoot a single device.
2. Confirm which account the user is signed into (Settings > Accounts > Access work or school, and inside the app: File > Account). Wrong-account sign-ins cause many "access denied" problems.
3. Test the web version (outlook.office.com, teams.microsoft.com, office.com). If the web works, the problem is the desktop client or the device. If the web fails too, it is the account, license or service.
4. Ask whether another user or device has the same issue to find the scope.

## 2. Sign-in and MFA

| Symptom | Likely cause | First action |
| --- | --- | --- |
| Repeated password prompts | Stale cached credentials or token | Sign out of Office apps, clear Credential Manager entries for `MicrosoftOffice16` and `Microsoft_OC1`, sign in again |
| "Your account is locked" | Too many bad passwords (Entra smart lockout or AD lockout) | Verify identity, check the sign-in logs, unlock per policy |
| MFA prompt never arrives | Authenticator notification or phone issue | Use a different method (number, TOTP), check the device clock and network, re-register the method if policy allows |
| Conditional Access block | Non-compliant device, location or app restriction | Read the error and correlation ID, check device compliance in Intune, escalate to identity admins |
| New device sign-in loop | Device not joined or registered | Check `dsregcmd /status` for `AzureAdJoined` and `AzureAdPrt` |

Never reset MFA or a password without verifying the user's identity using the documented procedure (see the Active Directory workflow).

## 3. Licensing

1. In the admin center, open Users > Active users > the user > Licenses and apps. Confirm a license that includes the needed service (Exchange, Teams, SharePoint, Office apps) is assigned.
2. Licenses can take several minutes to apply. Group-based licensing can show processing errors under Entra ID > Groups > Licenses.
3. A mailbox is created after Exchange Online is licensed. A user without it will see no mailbox.
4. Confirm the user is not blocked from sign-in and that the usage location is set (required before assigning licenses).

## 4. Outlook

**Will not start or stuck on "Loading profile"**

1. Start in safe mode: `outlook.exe /safe`. If it works, disable add-ins (File > Options > Add-ins).
2. Create a new profile: Control Panel > Mail > Show Profiles > Add. Test with the new profile before deleting the old.
3. Run the Microsoft Support and Recovery Assistant for persistent failures.

**Search is incomplete or slow**
Check that indexing is enabled (Windows Search service is running), then rebuild: Indexing Options > Advanced > Rebuild. Check File > Options > Search > Indexing Options.

**Not sending or receiving**

1. Check Work Offline (Send/Receive tab) is off, and the status bar shows Connected.
2. Look at the Outbox for a large attachment (limit is generally about 20-25 MB per message, then the message is rejected) and the Drafts folder.
3. Check the mailbox is not full (File > Info shows usage) and read any bounce message (NDR). The code, such as `5.7.1`, `5.1.1` or `5.2.2`, tells you where to look.
4. Test with `https://aka.ms/remoteconnectivityanalyzer` for connectivity and Autodiscover problems.

**Shared mailbox or calendar problems**
Confirm the permission (Full Access, Send As, Send on Behalf) in the Exchange admin center. Permission changes can take up to an hour to show in Outlook.

**Calendar mismatch**
Compare Outlook desktop with the web client. If the web is correct, repair the profile or the cached mode data (OST).

## 5. Microsoft Teams

| Symptom | Checks |
| --- | --- |
| Cannot sign in | Web client test, sign out completely, clear the Teams cache (new Teams: Settings > Apps > Installed apps > Teams > Advanced options > Reset) |
| No audio or video | Teams Settings > Devices, OS privacy settings for microphone and camera, correct default device, headset firmware, other apps using the device |
| Poor call quality | Wired connection or 5 GHz Wi-Fi, bandwidth, VPN split tunnel for Teams traffic, Call Quality Dashboard if you have access |
| Missing team or channel | Membership, private channel permissions, whether it was archived or deleted (a deleted team can be restored within 30 days by an admin) |
| Cannot share screen on macOS | System Settings > Privacy & Security > Screen Recording |

## 6. OneDrive sync

1. Click the OneDrive cloud icon and read the status message. A red X or exclamation mark shows the failing file.
2. Common causes: invalid characters or long paths, file open in an app, file over the size limit (250 GB), over the storage quota, or the account signed out.
3. Pause and resume sync. If it persists, **Reset OneDrive** (`%localappdata%\Microsoft\OneDrive\onedrive.exe /reset`). The reset does not delete files, but a full re-sync follows.
4. For "Known Folder Move" problems, check policy conflicts and that Desktop, Documents and Pictures are not redirected elsewhere.
5. For deleted files, use the OneDrive Recycle bin or SharePoint's second-stage Recycle bin (up to 93 days total), or version history on the file.
6. Files On-Demand shows cloud-only, locally available and always-keep-on-device states. Explain the icons to users.

## 7. Office app activation and errors

- "Unlicensed product" or activation loops: sign out of all Office apps, remove the work account under Account, and sign in again. Check the license assignment.
- Multiple installs (Click-to-Run plus older MSI) conflict. Uninstall the older version.
- Update channel and build: File > Account > About. Compare with the organization's standard.

## 8. Evidence to collect before escalating

User principal name, time and timezone of the failure, exact error with correlation or request ID, client and build version, web client result, scope (one user or many), steps already tried.

## Reference

Admin centers: Microsoft 365 admin center, Exchange admin center, Teams admin center, Entra admin center, Intune admin center. Free diagnostic tools: Microsoft Support and Recovery Assistant, Remote Connectivity Analyzer, `dsregcmd /status`.
