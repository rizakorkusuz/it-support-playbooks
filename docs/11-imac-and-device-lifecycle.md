# IMAC: Installs, Moves, Adds and Changes

IMAC work is the day-to-day movement of equipment and software: installing new items, moving them, adding to them and changing them. Each job is small. The risk is in what gets forgotten: records, testing and the user's own workflow. This page is a practical method for IMAC tickets. The full lifecycle (onboarding, loaners, RMA, disposal) is in the Device Lifecycle guide.

## 1. The five phases of every IMAC ticket

| Phase | Do | Record |
| --- | --- | --- |
| Request | Confirm what, for whom, where, by when, and who approved it | Ticket number, approval, due date |
| Prepare | Stage the equipment, approved image, software, licenses and cables | Asset tag and serial number |
| Execute | Install, move or change during the agreed window | Time, technician, steps |
| Verify | Test with the user's own tasks | Test results |
| Close | Update inventory and documentation, remove old equipment, get confirmation | Inventory change, final notes |

## 2. Install (new equipment or software)

1. Check the approved request, budget code if required, and the user's role.
2. Unbox and inspect. Record the asset tag and serial number in the inventory before the device leaves the bench.
3. Apply the approved standard image or build. Install updates, encryption and management agents.
4. Add only the approved applications. Check licensing before installing paid software.
5. Label the device and cables. Prepare the accessories (power supply, dock, keyboard, mouse, headset).
6. Deliver and connect. Verify identity before handing over anything that unlocks an account.
7. Test sign-in, email, the user's applications, printing, network and peripherals.
8. Brief the user for a few minutes (how to get help, what changed) and have them accept the equipment.

## 3. Move (relocation of people or equipment)

1. Get the old and new location, date and time, and who needs notice. Plan outside peak hours.
2. Before moving: note the current setup (monitors, dock, printers, mapped drives, phone extension if any) and back up data if the device will be offline for a long time.
3. Power off, label and disconnect cables so they can be reconnected in the same order.
4. Move devices safely. Use a cart for heavy equipment. Never leave cables across walkways. Lifting 30 pounds or more needs a cart or a second person.
5. At the new location confirm that the network port is active (link light, `ipconfig`), the power is available, and the printers are mapped.
6. Test, tidy the cables and update the inventory location field.
7. Confirm with the user, and remove packaging and trash.

## 4. Add (extra equipment or software)

Examples: a second monitor, a docking station, a headset, a software license, a shared printer connection.

1. Verify approval and compatibility (ports, cable type, OS version, license).
2. Install from the approved source only.
3. Test with the user's workflow, then document the setting or driver that worked.
4. Add the new item to the inventory with its assigned user.

## 5. Change (replace, upgrade or reconfigure)

1. Identify what changes and what must not change (data, settings, printers, certificates).
2. Confirm where the data lives (OneDrive Known Folder Move, network drive, local only). Protect local-only data before touching the device.
3. Build the replacement in advance. Swap during the agreed window.
4. Keep the old device until the user confirms that everything works.
5. Wipe the old device with the approved method, then return it to stock or send it for surplus. Keep the sanitization record.
6. Update the inventory for both devices and note the serial numbers in the ticket.

## 6. Verification checklist

- [ ] Sign-in works with the user's own account
- [ ] Email, Teams and other key applications open
- [ ] Network access works (wired or Wi-Fi), and the right printers are available
- [ ] Peripherals and displays work as the user needs
- [ ] Updates, encryption and management agents are present
- [ ] The user has tested and agreed that it works
- [ ] Inventory updated: owner, location, status, hostname
- [ ] Ticket updated with what was done, by whom, and when

## 7. Inventory hygiene

Keep a single source of truth with unique asset tags. Update the record at every IMAC. Reconcile with the management console regularly, and flag assigned devices without an owner, duplicates, expired warranties and devices not audited recently. A sample CSV, a validator and reporting scripts are in the `imac-asset-inventory-toolkit` repository.

## 8. Common IMAC problems

| Problem | Likely cause | Action |
| --- | --- | --- |
| No network after a move | Port not patched or disabled, wrong VLAN | Test another port, check link, request port activation |
| Wrong printer after a move | Printer mapped by location in old area | Remove old printers, add the nearest approved ones |
| Dual monitors wrong | Layout reset, wrong cable or port | Windows+P, Display settings, check dock firmware |
| Software missing after replacement | Not in the standard image | Install from the approved source, request an addition to the standard image |
| Data missing | Stored locally on old device | Recover from the old device that was kept, then back up properly |

## 9. Packaging and standard images (concepts)

Large organizations deploy standard images and software packages with management tools (for example Microsoft Configuration Manager, formerly SCCM, or Intune). A package contains the installer, install and uninstall commands, detection rules and requirements, and it is tested on a pilot group before wide release. Technicians test packages on a standard image in a lab, record results, and report defects. These are concept notes. Check the organization's own process and tools.
