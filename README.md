# it-support-playbooks

Practical, vendor-neutral runbooks for first- and second-line IT support: what to ask, what to check in which order, what to write in the ticket and when to escalate. Written as reference and study material and kept free of any employer or customer information.

**Roles this supports:** IT Support Specialist, Help Desk Specialist, Desktop Support Technician, Service Desk Analyst, Technical Support Specialist, Technical Support Engineer, IT Support Engineer, Deskside Support Technician, Field Service Technician.

## Playbooks

| # | Runbook | Covers |
| --- | --- | --- |
| 01 | [Windows 10/11 troubleshooting](docs/01-windows-troubleshooting.md) | Triage, slow PCs, SFC and DISM, updates, apps, BSOD, boot, profiles |
| 02 | [Microsoft 365, Outlook, Teams, OneDrive](docs/02-microsoft-365-troubleshooting.md) | Sign-in and MFA, licensing, Outlook, Teams, OneDrive sync |
| 03 | [Active Directory user admin and password reset](docs/03-active-directory-user-admin.md) | Identity verification, resets, lockouts, new users, groups, offboarding |
| 04 | [Network troubleshooting ladder](docs/04-network-troubleshooting-ladder.md) | Cable to application, Wi-Fi, VPN, command equivalents |
| 05 | [Printers and peripherals](docs/05-printers-and-peripherals.md) | Queues, spooler, secure print, docks, monitors, headsets |
| 06 | [Device lifecycle](docs/06-device-lifecycle.md) | Onboarding, offboarding, loaners, RMA, asset tracking, disposal |
| 07 | [Ticket writing, escalation and SLA matrix](docs/07-ticket-writing-and-escalation.md) | Templates, priority matrix, escalation rules |
| 08 | [Deskside and field service](docs/08-deskside-and-field-service.md) | Visit checklist, toolkit, safety, wrap-up |
| 09 | [macOS basics](docs/09-macos-basics.md) | Tools, common fixes, FileVault, privacy permissions, shortcuts |
| 10 | [Intune and Autopilot study notes](docs/10-intune-autopilot-study-notes.md) | Concepts and a troubleshooting map (study notes only) |

The same pages are published as a browsable guide at <https://rizakorkusuz.com/it-support/>.

## How to use

- Read a runbook before your shift, or open it while working a ticket. Each starts with triage questions and ends with evidence to collect.
- Adapt commands and policies to your organization. Where a step needs permission (resets, wipes, registry or service changes), get approval and follow your own change process.
- Treat the SLA numbers as examples, not standards.

## Accuracy and scope

- Commands target Windows 10/11, current macOS and Microsoft 365 as documented by the vendors. Product menus change, so verify against current vendor documentation.
- The Intune and Autopilot notes are **study notes**. They summarize documented behavior and are not a claim of production experience.
- No company confidential material, credentials or personal data are included. Examples use placeholder names.

## Companion repositories

- `windows-troubleshooting-scripts`: read-only PowerShell and batch diagnostics
- `service-desk-ticket-templates`: ticket, hand-off and knowledge base templates
- `device-lifecycle-checklists`: lifecycle checklists and an asset inventory CSV

## Contributing

Corrections are welcome through issues or pull requests. Please cite the vendor documentation for technical changes.

## License

MIT. See [LICENSE](LICENSE).
