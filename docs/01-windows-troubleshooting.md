# Windows 10/11 Troubleshooting Playbook

A repeatable order of operations for the most common Windows endpoint tickets: slow device, will not boot, app crashes, update failures, profile problems and blue screens. Use it as a checklist, not a script. Always confirm the symptom, scope and recent changes first.

## 1. Triage questions (60 seconds)

1. What exactly happens, and what should happen instead? Get the exact error text or a screenshot.
2. When did it last work? What changed (update, new software, password change, moved desks, new peripheral)?
3. Is it one user, one device, one application or one location?
4. Is there a workaround (another device, web version of the app)? Does the user's work stop completely?
5. Is the device company-managed, and is any data at risk (unsaved work, no backup)?

## 2. First-response checks

| Check | How | Why |
| --- | --- | --- |
| Uptime and pending restart | `systeminfo \| find "System Boot Time"` or Task Manager > Performance > Up time | Fast Startup means a "shutdown" may not clear the session. Restart, do not shut down. |
| Disk free space | `Get-PSDrive C` or Settings > System > Storage | Under about 10% free causes update, profile and performance trouble. |
| Resource hog | Task Manager > Processes, sort by CPU, Memory, Disk | Separates a runaway process from a hardware issue. |
| Time and date | Settings > Time & language | Wrong time breaks Kerberos, certificates and sign-in. |
| Recent updates | Settings > Windows Update > Update history | Correlates the start of the problem with a change. |

## 3. Slow performance

1. Check Task Manager for sustained 100% disk, CPU or memory use and identify the process.
2. Review Startup apps (Task Manager > Startup apps) and disable non-essential entries, with the user's agreement.
3. Check free disk space and clean up with Storage Sense or `cleanmgr`. Never delete user files without permission.
4. Check for a pending update or an active Windows Defender scan before assuming a fault.
5. Look at drive health (`Get-PhysicalDisk | Select FriendlyName,HealthStatus,OperationalStatus`). A failing drive means back up data first, then replace.
6. Check memory: 8 GB is a practical minimum for Windows 11 with Microsoft 365 apps and a browser with many tabs.
7. If the user's profile only is slow, test with a second profile. If both are slow, it is the device.

## 4. System file and image repair

Run from an elevated Command Prompt or PowerShell, in this order:

```
sfc /scannow
DISM /Online /Cleanup-Image /CheckHealth
DISM /Online /Cleanup-Image /ScanHealth
DISM /Online /Cleanup-Image /RestoreHealth
sfc /scannow
```

If `sfc` reports it cannot fix some files, run DISM `RestoreHealth` and then `sfc` again. Logs: `C:\Windows\Logs\CBS\CBS.log` and `C:\Windows\Logs\DISM\dism.log`.

## 5. Windows Update failures

1. Note the error code (for example `0x80070002`, `0x800f0922`, `0x80240034`) and search Microsoft documentation for that code.
2. Confirm free space (at least 20 GB for a feature update) and a stable connection.
3. Run the built-in Windows Update troubleshooter (Settings > System > Troubleshoot > Other troubleshooters).
4. Run the SFC and DISM sequence above.
5. Reset update components only as a last step: stop `wuauserv`, `cryptSvc`, `bits`, `msiserver`, rename `C:\Windows\SoftwareDistribution` and `C:\Windows\System32\catroot2`, then restart the services.
6. For managed devices, check whether Intune or WSUS policy controls updates before changing local settings. A local change may be overwritten or violate policy.
7. If a single update keeps failing, escalate with the error code, `CBS.log` excerpt and OS build (`winver`).

## 6. Application problems

1. Reproduce the issue, then check Event Viewer > Windows Logs > Application for Error entries at the same time (source `Application Error` or `.NET Runtime`).
2. Restart the app, then the device. Try Safe Mode or a clean sign-in to rule out startup conflicts.
3. Use Settings > Apps > Installed apps > Advanced options > Repair, then Reset (Reset deletes app data, so warn the user).
4. Check compatibility: app version against OS build, .NET and Visual C++ runtime requirements.
5. Reinstall from the approved software source (Company Portal, Software Center), not from random web downloads.

## 7. Blue screen (BSOD) basics

1. Record the stop code (for example `IRQL_NOT_LESS_OR_EQUAL`, `KERNEL_DATA_INPAGE_ERROR`, `CRITICAL_PROCESS_DIED`).
2. Check for recent driver, update or hardware changes. Boot into Safe Mode if the device loops.
3. Review `Get-WinEvent -FilterHashtable @{LogName='System';Id=41,1001}` for Kernel-Power 41 and BugCheck 1001 events. Minidumps are in `C:\Windows\Minidump`.
4. Update or roll back the suspected driver. Run `chkdsk /scan` and the Windows Memory Diagnostic (`mdsched.exe`).
5. Repeated crashes with different stop codes point to hardware (RAM, storage, overheating, power). Raise a hardware repair or replacement.

## 8. Will not boot

1. Confirm power and display first: charger LED, external monitor, different cable.
2. Hold the power button for 15 seconds, disconnect the dock and USB devices, then power on.
3. Enter firmware setup (usually F2, F10, Del or Esc by vendor) and confirm the drive is detected and boot mode is UEFI.
4. Boot Windows Recovery Environment (three failed boots, or installation media). Try Startup Repair, then System Restore, then `bootrec /fixmbr`, `bootrec /fixboot`, `bootrec /rebuildbcd` for BIOS/MBR cases. On UEFI systems `bootrec /fixboot` can return "Access is denied". In that case, rebuild the boot files with `bcdboot C:\Windows`.
5. If BitLocker prompts for a recovery key, retrieve it through the approved process (Entra ID, Active Directory or the MDM portal) after verifying the user's identity. Never ask users to send keys over chat.
6. If the drive is not detected, treat it as hardware failure and start the replacement and data-recovery process.

## 9. User profile problems

Symptoms: "We can't sign in to your account", temporary profile, missing desktop, or apps resetting at every sign-in.

1. Sign in with another account to confirm the device is healthy.
2. Check free space and that the account is not locked or expired.
3. For a corrupted local profile, back up `C:\Users\<name>`, then create a new profile and copy data back (Documents, Desktop, browser bookmarks, Outlook data if local).
4. For domain devices, remove the broken profile through System Properties > Advanced > User Profiles, and let it recreate at the next sign-in. Confirm the backup first.
5. Redirected folders and OneDrive Known Folder Move reduce risk. Confirm they are in place.

## 10. Close-out checklist

- Confirm with the user that the original problem is fixed and they can work.
- Record the root cause (or best known cause), steps taken and time spent in the ticket.
- Note any follow-up (replacement parts, pending reboot, KB article to write).
- Record anything changed on the device so the next technician can see it.

## Tools worth knowing

`Task Manager`, `Resource Monitor` (`resmon`), `Reliability Monitor` (`perfmon /rel`), `Event Viewer` (`eventvwr`), `msinfo32`, `winver`, `dxdiag`, `Get-ComputerInfo`, `Get-WinEvent`, `sfc`, `DISM`, `chkdsk`, `mdsched`, `Safe Mode`, `Windows RE`.
