# Microsoft Intune and Windows Autopilot: Study Notes

> **These are study notes.** They summarize how the products are documented to work, so I can learn the concepts and explain them. They are not a claim of production experience. Always confirm details in the current Microsoft Learn documentation, because the products change often.

## 1. What problem do they solve?

Organizations need to deploy, configure, secure and retire many devices without touching each one. **Microsoft Intune** (part of the Microsoft Intune product family) is a cloud-based mobile device management (MDM) and mobile application management (MAM) service. **Windows Autopilot** uses cloud services to deliver a pre-configured device out of the box so the user can sign in and get a managed device without IT imaging it.

## 2. Key terms

| Term | Meaning |
| --- | --- |
| MDM | Management using the OS's built-in management channel to apply settings and apps |
| MAM | Protecting data in apps (app protection policies) without full device enrollment, often for BYOD |
| Microsoft Entra ID | Cloud identity service (formerly Azure AD). Holds users, groups and devices |
| Entra join | Device is joined to the cloud directory. Hybrid join means joined to on-premises AD and registered in Entra |
| Enrollment | Registering a device with Intune so it can be managed |
| Compliance policy | Rules a device must meet (encryption, OS version, password, Defender status). Output: compliant or not |
| Configuration profile | Settings pushed to a device (Wi-Fi, VPN, certificates, restrictions, security baselines, settings catalog) |
| Conditional Access | Entra rules that grant or block access to resources based on user, device compliance, location and risk |
| Assignment, filters and groups | Who or what receives a policy or app. Assign to groups, use filters to refine |
| Company Portal | App users use to install approved apps and check device status |
| Win32 app packaging | `.intunewin` format for packaging complex installers, with detection rules |
| Update rings and feature updates | Windows Update for Business policies in Intune controlling deferral and deadlines |
| Hardware hash | Device identifier uploaded to register the device with Autopilot |

## 3. Autopilot deployment modes (high level)

| Mode | Idea |
| --- | --- |
| User-driven | User signs in during setup. Device joins Entra (or hybrid) and applies the profile. The most common mode |
| Self-deploying | No user affiliation, for kiosks and shared devices. Needs TPM 2.0 attestation |
| Pre-provisioning (formerly white glove) | IT or a partner does the technician phase in advance, and the user completes only the user phase |
| Autopilot reset | Resets a device and returns it to a business-ready state |
| Autopilot device preparation | Newer, simplified approach for user-driven deployments, which uses device groups and policies. Check current documentation for availability and limits |

## 4. Typical user-driven flow

1. Hardware hash is registered (by the OEM or reseller, or imported by IT).
2. The device is assigned an Autopilot deployment profile through a group.
3. User unboxes and connects to the network, and the device contacts Autopilot services.
4. The user signs in with work credentials, MFA when required, and the device joins Entra ID and enrolls into Intune automatically.
5. The Enrollment Status Page (ESP) can block until required apps and policies are installed.
6. Policies, apps and compliance evaluation are applied, and the user reaches the desktop.

## 5. Troubleshooting map (concepts)

| Symptom | Where to look |
| --- | --- |
| Device does not get the Autopilot profile | Hardware hash registered? Profile assigned through a group? Sync completed? Network reachable to the required Microsoft endpoints? |
| ESP stuck or times out | Which app or policy is blocking it? Review the ESP tracking in Intune, device logs, app install status and the timeout settings |
| Device shows non-compliant | Which compliance setting failed (BitLocker, OS version, firewall, Defender)? Device > Device compliance in Intune |
| App not installing | Assignment (required or available), detection rule, requirement rule, install command, return codes. IME logs in `C:\ProgramData\Microsoft\IntuneManagementExtension\Logs` |
| Policy not applying | Assignment group, filters, conflicts with another policy or GPO (for co-managed or hybrid devices), device check-in time |
| Sync | Settings > Accounts > Access work or school > Info > Sync, or the Sync action in the Intune console |
| Wipe, Retire, Delete | Wipe resets the device and removes all data. Retire removes company data and management. Delete removes the record from Intune only. Know the difference before using each |
| Lost device | Mark lost or use Lock, Locate (where supported), and escalate to security. BitLocker and Conditional Access reduce risk |

Useful local commands and tools: `dsregcmd /status`, `mdmdiagnosticstool.exe`, Event Viewer > Applications and Services Logs > Microsoft > Windows > DeviceManagement-Enterprise-Diagnostics-Provider, and the Intune management extension logs.

## 6. How Intune relates to daily service desk work

- Checking whether the device is enrolled, compliant, encrypted and which user is assigned.
- Retrieving the BitLocker recovery key (with the right role and verification).
- Triggering a sync, restarting or running a remote action such as Wipe or Autopilot Reset, with approval.
- Pushing or fixing an app deployment through Company Portal.
- Distinguishing a device problem from a policy or identity problem.
- Role-based access control: support staff get only the roles they need (Help Desk Operator is a built-in Intune role).

## 7. Study questions to test myself

1. What is the difference between MDM and MAM, and when would you use each?
2. Name the Autopilot deployment modes and when each fits.
3. A new laptop is stuck on the Enrollment Status Page. What do you check first?
4. What is the difference between Wipe, Retire and Delete in Intune?
5. How does a compliance policy interact with Conditional Access?
6. What is a hardware hash and who normally registers it?
7. What must happen before you wipe a Mac enrolled with Apple Business Manager?
8. A user says an app is not on the Company Portal. What assignment choices explain it?

## 8. Official references

Microsoft Learn: Intune documentation, Windows Autopilot documentation, Microsoft Entra documentation, and the Intune troubleshooting guidance. Use the current version of the documentation as the source of truth.
