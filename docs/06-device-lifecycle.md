# Device Lifecycle: Onboarding, Offboarding, Loaners, RMA and Asset Tracking

A device moves through a predictable life: request, procurement, provisioning, assignment, support, refresh or return, and secure disposal. Good records at every step make audits, security and support much easier. Checklists in this page are templates. Adapt them to your organization's policies.

## 1. Lifecycle stages

| Stage | Key actions | Record in asset system |
| --- | --- | --- |
| Request and approval | Approved ticket with role, user, start date | Request number, cost center |
| Receive and stock | Check against the order, inspect, label | Asset tag, serial number, model, purchase date, warranty end |
| Provision | Enroll, image or Autopilot, encrypt, install baseline apps, update | Hostname, OS build, management status |
| Assign | Hand over, user acknowledges | Owner, location, assignment date, accessories |
| Support | Repairs, loaners, upgrades | Incident links, parts used |
| Return or refresh | Collect, verify condition, wipe | Return date, condition, status |
| Dispose | Certified wipe, recycle or resale | Certificate of data destruction, disposal date |

## 2. New-hire device onboarding checklist

**Before day one**

- [ ] Approved request linked in the ticket (name, role, start date, manager, location)
- [ ] Device allocated and the asset tag recorded
- [ ] Device enrolled in management (Intune/Autopilot or imaging process) and named per convention
- [ ] BitLocker (Windows) or FileVault (macOS) enabled, with the recovery key escrowed
- [ ] Operating system and firmware up to date, baseline apps installed
- [ ] Security agents present (endpoint protection, management agent)
- [ ] User account, mailbox, license and group memberships ready
- [ ] Accessories ready: charger, dock, monitor, mouse, keyboard, headset, bag

**Day one**

- [ ] Verify the identity of the new user in person before handing over
- [ ] Sign-in with a temporary password and forced change, set up MFA
- [ ] Confirm email, Teams, OneDrive, VPN, printers, shared drives and line-of-business apps work
- [ ] Walk through how to get help (portal, phone, walk-up hours)
- [ ] User signs the equipment acknowledgment or accepts it in the system
- [ ] Update the asset record: assigned user, date, location

**After day one**

- [ ] Follow up within a few days to catch missing access or app issues
- [ ] Close the ticket with a summary

## 3. Offboarding and equipment return checklist

- [ ] Received an approved offboarding request with effective date and timing
- [ ] Account disabled at the specified time, sessions revoked, MFA methods cleared if policy says so
- [ ] Data handling arranged with the manager (mailbox, OneDrive, files), within the retention policy
- [ ] Equipment return arranged (in person, or prepaid shipping for remote staff, with tracking number)
- [ ] Items received and counted against the assignment list: laptop, charger, dock, peripherals, badge, phone, tokens
- [ ] Physical inspection and condition noted, with photos for damage
- [ ] Device removed from management as required, and the recovery keys handled according to policy
- [ ] Device **wiped** with the approved method (Intune Wipe or Retire, Autopilot Reset, or reinstall) and verified
- [ ] Device returned to stock or queued for repair or disposal, and the asset record updated
- [ ] Licenses reclaimed and distribution lists adjusted after the retention period
- [ ] Ticket closed with the return confirmation

## 4. Loaner device process

1. Check that a loaner is justified (repair in progress, urgent travel, replacement lead time).
2. Use a dedicated pool of loaners: pre-imaged, encrypted, up to date and with no stored user data.
3. Record in the loaner log: asset tag, user, date out, expected return date, accessories, condition, linked repair ticket.
4. Remind the user that data on the loaner should be in OneDrive or corporate storage, and that they must not store sensitive data locally.
5. On return, inspect, **wipe or reset**, update, and put it back in the pool. Close the log entry.
6. Follow up on overdue loaners.

## 5. Repair, warranty and RMA process

1. Diagnose and capture evidence: serial number, error codes, diagnostics output, photos, steps already tried.
2. Check warranty status on the manufacturer's site with the serial number. Note the service level (next business day, onsite, mail-in).
3. Open the case with the vendor and record the case number in the ticket and the asset record.
4. Protect data: back up if possible, and for mail-in repairs remove or encrypt storage according to policy. Keep the user's drive when allowed.
5. Provide a loaner and communicate expected timeline.
6. Track the shipment (outbound and return) and note receipt of the replacement or repaired unit. Verify the serial number matches or the replacement is recorded as a new asset.
7. Reprovision as needed, return to the user and close the case.
8. Record the failure category so you can see patterns (batteries, docks, a model-wide problem).

## 6. Asset tracking basics

**Minimum fields**: asset tag, serial number, make and model, device type, purchase date, vendor, warranty end date, status (in stock, assigned, loaner, in repair, retired, disposed), assigned user, department, location, hostname, operating system, encryption status, last audit date, and notes.

Practices:

- One unique asset tag per device, physically labeled, never reused.
- Update the record at every handoff. A device without a current owner is a risk.
- Reconcile the asset list against the management console (Intune, AD, EDR) regularly to find unmanaged or missing devices.
- Run a periodic physical audit for high-value or portable items.
- Track consumables and accessories at least by quantity and reorder point.
- Use status values consistently (a short controlled list) so reports work.

A starter inventory CSV and the checklists are in the `device-lifecycle-checklists` repository.

## 7. Refresh planning

- Typical laptop refresh cycles are three to five years, depending on usage and budget. Use warranty end, battery health, performance, OS support (Windows 10 support ended in October 2025 for most editions) and failure rates.
- Build a report of devices past the refresh age and those that fail the Windows 11 hardware requirements (TPM 2.0, Secure Boot capable, supported CPU, 4 GB RAM, 64 GB storage).
- Plan data migration (OneDrive Known Folder Move reduces effort), application compatibility and a rollout schedule that avoids busy periods.

## 8. Secure disposal

- Use a certified wipe (NIST SP 800-88 media sanitization guidelines) or physical destruction for failed drives.
- Remove the device from management, directory and monitoring tools. Remove asset tags if required.
- Keep the certificate of destruction or recycling with the asset record.
- Do not donate or sell devices that have not been verified as wiped.
