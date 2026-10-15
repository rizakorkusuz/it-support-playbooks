# macOS Basics Cheat Sheet for Support

For a mixed Windows and Mac environment. Menu names follow macOS Ventura and later (System Settings). Older versions use System Preferences. Check the version first (Apple menu > About This Mac).

## 1. First checks

| Item | How |
| --- | --- |
| macOS version and model | Apple menu > About This Mac, or `sw_vers` and `system_profiler SPHardwareDataType` |
| Serial number | About This Mac, or `system_profiler SPHardwareDataType \| grep Serial` |
| Apple silicon or Intel | About This Mac (Chip or Processor) |
| Free storage | System Settings > General > Storage, or `df -h /` |
| Uptime | `uptime` |
| Pending updates | System Settings > General > Software Update |

## 2. Everyday tools

- **Activity Monitor**: CPU, Memory, Energy, Disk, Network. Equivalent of Task Manager.
- **Force Quit**: Option+Command+Esc.
- **Spotlight**: Command+Space.
- **Console**: system and app logs. **Disk Utility**: drive health and First Aid.
- **Terminal**: shell. Useful commands include `ifconfig`, `networksetup`, `ping`, `dig`, `top`, `diskutil list`, `softwareupdate -l`, `defaults`, `profiles list`.
- **Keychain Access**: stored passwords and certificates.
- **Screenshot**: Shift+Command+5.

## 3. Common problems

**Slow Mac**

1. Activity Monitor: find the process using CPU or memory. Check Memory Pressure (green, yellow, red).
2. Check free disk space, since less than about 10% free hurts performance.
3. Reduce login items (System Settings > General > Login Items).
4. Update macOS and apps, and restart. Check for runaway browser extensions.

**App not responding or crashes**: Force Quit, reopen, update the app, check Console for crash reports, and reinstall from the approved source. Check that Rosetta is installed for Intel-only apps on Apple silicon (`softwareupdate --install-rosetta`).

**Wi-Fi problems**: toggle Wi-Fi, check the network in System Settings, forget and rejoin the network, renew the DHCP lease, check DNS. Hold Option and click the Wi-Fi icon for diagnostics. Wireless Diagnostics can create reports.

**Cannot print**: System Settings > Printers & Scanners. Remove and add the printer using the right driver or IP/AirPrint. To reset the printing system, right-click (Control-click) the printer list and choose "Reset printing system" (removes all printers).

**Bluetooth devices**: turn Bluetooth off and on, remove and re-pair the device.

**Will not boot or frozen**: hold the power button for 10 seconds. On Apple silicon, hold the power button at startup to enter startup options (Recovery). On Intel, use Command+R at startup for Recovery, or Option for the startup disk picker. On Intel Macs only, resetting NVRAM (Option+Command+P+R) and the SMC can help.

**Disk errors**: Disk Utility > First Aid on the volume, then the container, then the disk.

## 4. Accounts and security

- **FileVault**: full-disk encryption. Check with `fdesetup status`. For managed Macs the recovery key is usually escrowed in the MDM. Never store the key in the ticket.
- **Gatekeeper and notarization**: block unidentified apps. Do not disable them as a quick fix.
- **Privacy permissions** (System Settings > Privacy & Security): screen recording, accessibility, camera, microphone and full disk access are common causes of "Teams cannot share my screen" or "app cannot see files".
- **Firewall and XProtect**: built-in. Check status in System Settings.
- **Local admin**: follow policy. Many organizations use standard users with just-in-time elevation.
- **Activation Lock**: tied to an Apple Account. Managed devices should use Activation Lock bypass through the MDM. Must be disabled before wiping a device for reuse.
- **Password and Apple Account**: macOS login password can be linked to a directory or identity provider. Resets differ by setup. Follow the documented process.

## 5. Mac in a Microsoft environment

- Microsoft 365 apps for Mac, OneDrive, Teams and Outlook work well. Sign in with the work account, and watch the OneDrive Files On-Demand folder in `~/Library/CloudStorage`.
- Join to the corporate identity with Microsoft Enterprise SSO plug-in or Platform SSO where configured.
- Connect to Windows file shares with `smb://server/share` (Finder > Go > Connect to Server).
- Management is usually through an MDM (Microsoft Intune, Jamf and others) using Apple Business Manager and Automated Device Enrollment.
- Format external drives as exFAT when sharing between Mac and Windows.

## 6. Keyboard shortcut translation

| Task | Windows | macOS |
| --- | --- | --- |
| Copy / paste / cut | Ctrl+C / V / X | Command+C / V / X |
| Switch apps | Alt+Tab | Command+Tab |
| Task manager | Ctrl+Shift+Esc | Option+Command+Esc |
| Lock screen | Windows+L | Control+Command+Q |
| Screenshot | Windows+Shift+S | Shift+Command+4 or 5 |
| Search | Windows key | Command+Space |
| Rename file | F2 | Return |
| Delete file | Delete | Command+Delete |
| Properties / Get Info | Alt+Enter | Command+I |
| Show desktop | Windows+D | F11 or Fn+F11 |

## 7. Useful Terminal commands

```
sw_vers                          # macOS version
system_profiler SPHardwareDataType
diskutil list                    # disks and volumes
softwareupdate -l                # list available updates
networksetup -listallhardwareports
scutil --dns                     # DNS configuration
sudo dscacheutil -flushcache; sudo killall -HUP mDNSResponder
fdesetup status                  # FileVault status
profiles list                    # installed configuration profiles (may need sudo)
```

Commands marked with `sudo` change the system. Understand them before use and follow policy on managed devices.
