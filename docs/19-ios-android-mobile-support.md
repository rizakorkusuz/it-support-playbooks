# iOS, Android and Mobile Device Support

Phones and tablets are now part of the standard support workload: email and Teams on a phone, mobile hotspots, field tablets and managed devices. This guide covers triage, common fixes and the concepts of mobile device management. The macOS side is in the macOS Basics guide.

## 1. Triage questions

1. Which device and OS version (Settings > General > About on iPhone and iPad, Settings > About phone on Android)?
2. Is the device personally owned or organization-owned and managed?
3. What exactly fails (email, Wi-Fi, calls, an app, battery) and since when?
4. Does it fail on cellular, Wi-Fi or both? Do other devices on the same network work?
5. Did anything change (OS update, new app, password change)?

## 2. First steps for most problems

- Restart the device. Force restart if it is frozen (steps differ by model).
- Check for iOS or Android and app updates, and storage space (low space causes many faults).
- Toggle Airplane mode, then Wi-Fi and Bluetooth.
- Test in another location or network to separate the device from the network.

## 3. Wi-Fi and connectivity

| Symptom | Checks |
| --- | --- |
| Will not join Wi-Fi | Correct password or enterprise credentials, forget the network and rejoin, check for a certificate prompt, restart the router if you own it |
| Connected but no internet | Open a browser for a captive portal sign-in, test cellular, check DNS or VPN |
| Weak signal | Move closer, test other devices, check Wi-Fi band support |
| Private or randomized MAC address | Some networks restrict by MAC address. Use the network's fixed address option (iOS: Private Wi-Fi Address, Android: MAC type) if the network owner requires it |
| Hotspot fails | Plan allowance, compatibility mode on iPhone, band and security settings |

## 4. Email, accounts and Microsoft 365

1. Confirm the account and password work on the web (outlook.office.com).
2. Remove and re-add the account on the phone, or sign in again in the Outlook app.
3. For repeated password prompts, check for an expired password, MFA method changes and whether the device is compliant (conditional access).
4. Teams: check notifications permissions, background app refresh or battery optimization, and sign out and in.
5. Sign-in problems after a password change usually need the new password entered once on each device.

## 5. Common device faults

| Fault | Actions |
| --- | --- |
| Battery drains quickly | Check battery health (iPhone: Settings > Battery > Battery Health; Android: vendor tools), find apps using power, reduce screen brightness and background activity, replace an aged battery |
| Device is slow or storage is full | Remove unused apps and old media, clear app cache (Android), back up first |
| App crashes | Update or reinstall the app, restart, check OS compatibility |
| Charging problems | Try a different cable and adapter, clean the port gently with a non-metal tool, test wireless charging |
| Cracked screen or water damage | Create a repair ticket, protect data, follow the warranty or insurance process |
| Bluetooth pairing fails | Remove the pairing on both sides, restart, pair again, update firmware |
| Cannot make calls | Check SIM, carrier signal, Airplane mode, Wi-Fi calling settings, carrier outage |

## 6. Apple-specific points

- **Apple Account** and Activation Lock tie a device to its owner. A device with Activation Lock cannot be reused without the owner's credentials or an MDM bypass for managed devices.
- Updates: Settings > General > Software Update. Keep a backup first (iCloud or a Mac).
- Backup and restore: iCloud Backup, or Finder on a Mac.
- Reset steps range from restart to **Erase All Content and Settings**. Use the erase option only with approval.

## 7. Android-specific points

- Manufacturers customize menus. Search Settings for the option name.
- **Safe mode** (hold the power-off option) helps find a misbehaving app.
- Google account sign-in and Play Protect affect app installs. Work profiles keep corporate data separate on personal phones.
- Clear an app's cache before clearing its data. Clearing data deletes its stored information.

## 8. Mobile device management (concepts)

MDM lets an organization enroll devices, enforce a passcode and encryption, deploy apps and settings, check compliance, locate or lock lost devices and remotely remove company data or wipe a device.

| Term | Meaning |
| --- | --- |
| Enrollment | Registering a device with management. Apple Business Manager and Android Enterprise provide automated enrollment |
| Supervised or fully managed | Organization-owned devices with more control |
| BYOD and work profile | Personal devices with a separated work area. Only company data can be removed |
| Compliance policy | Rules the device must meet to get access (passcode, OS version, encryption) |
| Retire vs wipe | Retire removes company data. Wipe erases the entire device |

Support tasks: check the device is enrolled and compliant, trigger a sync, explain why a policy blocks access and escalate remote wipes or lost-device actions according to the security procedure. Concepts only: this guide does not claim hands-on use of a particular MDM product.

## 9. Lost or stolen devices

Treat as a security incident. Record the device and last location, lock or locate it using approved tools, change the user's passwords, report to security and the carrier, and follow the wipe policy.

## 10. Security and user guidance

Keep devices updated, use a passcode and biometrics, install apps only from official stores, be careful with links and attachments in messages, and turn on backups. Never ask users to share passcodes.

## 11. Closing the ticket

Confirm the original task works (send an email, join Teams, print), record the device model and OS, steps taken and any follow-up, and update the asset record if hardware changed.
