# it-support-playbooks

Practical, vendor-neutral runbooks for first- and second-line IT support: what to ask, what to check in which order, what to write in the ticket and when to escalate. Written as reference and study material and kept free of any employer or customer information.

**Roles this supports:** Information Systems Technician, IT Support Specialist, Help Desk Specialist, Desktop Support Technician, Service Desk Analyst, Technical Support Specialist, Technical Support Engineer, IT Support Engineer, Deskside Support Technician, Field Service Technician.

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
| 11 | [IMAC: installs, moves, adds and changes](docs/11-imac-and-device-lifecycle.md) | Five-phase IMAC method, verification checklist, inventory hygiene |
| 12 | [Knowledge base and documentation](docs/12-knowledge-base-and-documentation.md) | Writing a service desk wiki, nine sample articles |
| 13 | [Wired and wireless networks](docs/13-wired-and-wireless-networks.md) | Ownership boundaries, wired and Wi-Fi troubleshooting, documentation |
| 14 | [Audio-visual and conference rooms](docs/14-av-and-conference-room-support.md) | Room sheets, pre-meeting check, symptom table, vendors |
| 15 | [User training guides](docs/15-user-training-guides.md) | One-on-one method and quick guides |
| 16 | [Web basics: HTML, JavaScript, XML](docs/16-web-basics-html-js.md) | Dev tools, troubleshooting table, HTTP codes, XML basics |
| 17 | [Microsoft Access basics](docs/17-ms-access-basics.md) | Tables, queries, forms and reports for support staff |
| 18 | [Excel reporting and data queries](docs/18-excel-reporting-and-data-queries.md) | Reports, lookups, pivots and data queries |
| 19 | [iOS, Android and mobile support](docs/19-ios-android-mobile-support.md) | Mobile device setup and troubleshooting |
| 20 | [HTML, DHTML and XML basics](docs/20-html-xml-basics.md) | Reading and troubleshooting markup |
| 21 | [Software deployment and imaging](docs/21-software-deployment-and-imaging.md) | SCCM-style concepts (study notes) |
| 22 | [Enterprise systems, Business Objects and GIS](docs/22-enterprise-systems-and-gis-concepts.md) | Concepts only (study notes) |

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
