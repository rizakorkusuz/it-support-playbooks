# Printer and Peripheral Troubleshooting

Printers cause a large share of walk-up and ticket volume. The same method works for docks, monitors, headsets and other peripherals: find out whether the fault is the device, the connection, the driver or the queue.

## 1. Printer triage

1. Which printer (name or asset tag), which user, which application? One document or everything?
2. Does the printer have a message on its panel (paper jam, toner, tray, error code)?
3. Does it print a test page from the printer's own menu? Yes means the printer hardware is fine.
4. Do others print to it? If yes, the problem is the user's device or queue. If no, check the printer and the print server.

## 2. Connectivity checks

- Network printers: confirm the IP address (print a configuration page) and `ping` it. Use `Test-NetConnection <ip> -Port 9100` for raw printing, or port 631 for IPP.
- Make sure the printer's address is **static or reserved in DHCP**. A changed address is a common cause of "it worked yesterday."
- USB printers: try another cable and port, avoid unpowered hubs, and check Device Manager for errors.
- VLAN and firewall: users on a different subnet need an allowed path to the printer or print server.

## 3. Windows print queue problems

**Jobs stuck in the queue**

1. Open Settings > Printers & scanners > the printer > Open print queue. Cancel the jobs.
2. If they will not clear, restart the Print Spooler service:

```
net stop spooler
del /Q /F /S "%systemroot%\System32\spool\PRINTERS\*.*"
net start spooler
```

Only delete spool files after cancelling the jobs, and with administrator rights.

**Printer shows Offline**
Check the printer is on and connected, the port points to the right IP (Printer properties > Ports), and that "Use Printer Offline" is not ticked. Remove and re-add the printer when the port or driver is wrong.

**Wrong output or garbled text**
Use the correct driver for the model (the manufacturer's universal driver for fleets), and check the paper size, tray and duplex settings.

**Error 0x0000011b, 0x00000709 or 0x00000bcb after updates**
These are linked to print security changes (RPC authentication and Point and Print restrictions). Do not disable hardening as a permanent fix. Review Microsoft's documentation for the specific KB and apply the supported configuration, and escalate to the print or endpoint team.

## 4. Print servers and secure print

- Shared queues are normally hosted on a Windows print server and deployed by Group Policy or Intune. Check whether the queue is reachable and the spooler is running on the server.
- Many organizations use a print management product (for example PaperCut) for **secure release printing**, quotas and reporting. Typical user issues: job not showing at the release station because the user printed to the wrong queue, the account name or card is not registered, the job expired, or the client is not signed in. Check the job log in the management console (if you have access) to confirm whether the job reached the server.
- Mobile and guest printing use separate queues or email submission. Follow the local documentation.

## 5. Hardware faults

| Symptom | Common cause | Action |
| --- | --- | --- |
| Paper jams | Wrong or damp paper, worn rollers, debris | Clear the jam following the on-screen path, check the paper type, inspect rollers, log recurring jams |
| Streaks or faded print | Low toner, dirty or worn drum or fuser | Replace the toner, run a cleaning cycle, raise a service request |
| Error code on panel | Fuser, sensor or firmware error | Look up the model's code, power cycle, call the vendor if it persists |
| Will not scan to email or folder | Credentials or SMTP/SMB settings changed | Check the stored service account, server name and share permissions |

Follow the maintenance and warranty process. Do not open fuser units while hot and avoid unauthorized repairs on managed devices.

## 6. Docks, monitors and USB

1. Reseat all cables. Test with another cable and another port, then with another dock or monitor to isolate the part.
2. Check that the dock's firmware and display drivers are current (vendor tool or Windows Update).
3. Display issues: Windows+P to confirm the projection mode, Settings > System > Display to detect, set the resolution and refresh rate, and use the right cable type (DisplayPort or HDMI version limits).
4. USB devices that disconnect: disable USB selective suspend in the power plan, try a powered hub.
5. Ethernet through the dock: confirm the dock's MAC address pass-through if network access control uses MAC addresses.
6. Peripherals that stop only after sleep: update drivers and check the power management tab in Device Manager.

## 7. Headsets, webcams and audio

1. Settings > System > Sound: select the right input and output.
2. Check privacy permissions for microphone and camera, and that no other app has locked the device.
3. In Teams or Zoom, choose the device in the app's own settings.
4. Test on another computer to isolate the device from the OS.
5. Update the firmware of Bluetooth or USB headsets, and re-pair Bluetooth devices after removing them.

## 8. Mouse, keyboard and Bluetooth

Replace batteries first, then re-seat the dongle in another port, re-pair, and check for interference. Remove the Bluetooth device from Settings and pair again. For managed devices, check whether policy blocks the device.

## 9. Before closing the ticket

- Print a test page from the user's device and ask them to print their real document.
- Record the printer name, the driver and the port that worked.
- If you replaced hardware, update the asset record.
