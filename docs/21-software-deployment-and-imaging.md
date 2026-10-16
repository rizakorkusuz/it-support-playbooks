# Software Deployment and Imaging (SCCM-style Concepts)

> **Study notes.** These notes summarize how Windows imaging and remote software deployment are documented to work, including the model used by Microsoft Configuration Manager (formerly SCCM, also called MECM or ConfigMgr). They are not a claim of production experience with any of those products. Check current Microsoft documentation and your organization's own process.

## 1. Why deploy centrally?

Installing software and building computers by hand does not scale and creates inconsistencies. Central deployment gives a standard image, repeatable software installs, a record of what is installed where, patching, and the ability to fix many devices at once.

## 2. Standard image

A **standard image** is a tested Windows build containing the operating system, drivers, baseline security settings, and the applications everyone needs. Departments add their own applications on top.

| Approach | Description |
| --- | --- |
| Thick image | Many applications baked into the image. Fast to deploy, harder to keep current |
| Thin image | Minimal operating system, applications added after. Easier to maintain |
| Provisioning (modern) | Device enrolled with Windows Autopilot, apps and policies applied by management after sign-in |

Images and builds must be updated regularly for security patches and driver changes, and tested on each hardware model before release.

## 3. How imaging works (concept)

1. The device boots from the network (PXE) or from USB media into a deployment environment (Windows PE).
2. A **task sequence** runs a series of steps: partition and format disks, apply the OS image, inject drivers for the model, join the domain, install applications and run updates.
3. The device restarts into Windows, and management agents report back.

Common failures: no DHCP or PXE response, wrong boot mode (UEFI or legacy), drivers missing for the network card or storage controller, domain join or naming problems, and application steps failing.

## 4. Software packaging and deployment (concept)

In an SCCM-style model the main objects are:

| Object | Meaning |
| --- | --- |
| Application | An installer plus detection rules, requirements and install and uninstall commands |
| Package and program | An older, simpler model: a source folder and a command line |
| Deployment | An assignment of an application to a **collection** of users or devices, as Required or Available |
| Collection | A group of devices or users, defined by membership rules or queries |
| Distribution point | A server that stores content so clients download from a nearby location |
| Client agent | Software on the device that checks in, downloads and runs deployments, and reports inventory |
| Software Center | The tool users open to install available applications and see required ones |
| Task sequence | A scripted series of steps, used for imaging and complex installs |

### Packaging checklist

1. Identify the installer (MSI, EXE, MSIX) and silent switches (`msiexec /i app.msi /qn`, vendor `/S` or `/quiet`).
2. Write an install command, an uninstall command and a **detection rule** (file version, registry key or MSI product code) so the system knows it worked.
3. Set requirements (OS version, free space, architecture).
4. Test on a clean standard image in a lab, then on a small pilot group, then widen.
5. Record the package version, source location, license and contact.
6. Document how users get help.

### Example silent install checks

```
msiexec /i "App.msi" /qn /norestart /l*v "C:\Windows\Temp\App_install.log"
echo %ERRORLEVEL%
```

Exit code `0` means success and `3010` means success but restart required. Other codes mean errors. Read the verbose log for the failing step.

## 5. Remote software deployment and troubleshooting

| Symptom | What to check |
| --- | --- |
| App never appears in Software Center | Device in the right collection, deployment targeted to users or devices, client policy refresh, requirements not met |
| Install fails | Exit code, install log, disk space, running app or reboot pending, correct architecture, detection rule |
| "Installed" but app is missing | Detection rule wrong or app removed later |
| Download stalls | Network, distribution point availability, bandwidth limits |
| Client not checking in | Client service stopped, time or DNS problems, certificate or firewall issue |
| Deployment applies to the wrong devices | Collection membership query or scope |

Useful local items on Windows: Software Center, Event Viewer, installer logs under `C:\Windows\Temp` or the product's log folder, `Get-HotFix`, `Get-WmiObject Win32_Product` (avoid for routine use because it can trigger repairs) or `Get-Package`, and `winget` where approved.

## 6. Patching

Operating system patches are normally approved, tested on a pilot ring and released in waves. Support staff check pending reboots, report failed updates with the error code, and avoid disabling update services.

## 7. Related tools and modern equivalents

| Need | Examples |
| --- | --- |
| Cloud device management | Microsoft Intune (see the Intune and Autopilot study notes) |
| Provisioning | Windows Autopilot |
| Windows packages | MSI, MSIX, winget, Chocolatey in labs |
| Remote support | Remote Assistance, Quick Assist, vendor remote tools (with the user's consent) |
| Scripts | PowerShell scripts to check or install, run through management tools |
| macOS | MDM with Apple Business Manager, installer packages (`.pkg`) |

## 8. Testing and change control

- Test packages in a lab on the standard image before any user sees them.
- Use a pilot group, then staged release.
- Keep a rollback or uninstall plan.
- Record changes in the change process and notify users.

## 9. What a technician contributes

Testing packages and scripts and reporting results clearly, documenting installs and known issues, identifying devices that fail to receive software, escalating deployment or server problems to the owner, and keeping inventory records aligned with what is installed.

## 10. Study questions

1. What is the difference between an application and a package?
2. Why is a detection rule important?
3. What do exit codes 0 and 3010 mean?
4. A new application does not appear in Software Center. What do you check first?
5. Why test on a standard image before a pilot?
