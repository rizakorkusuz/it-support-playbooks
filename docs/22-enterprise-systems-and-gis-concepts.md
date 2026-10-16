# Enterprise Systems, Data Warehouses, Business Objects and GIS: Concepts

> **Willing to learn, not claiming experience.** These are concept notes about the kinds of business systems a government IT technician supports: enterprise resource planning (such as SAP), permitting and land-management systems, data warehouses, reporting tools (such as SAP BusinessObjects and Crystal Reports) and geographic information systems (GIS). I have not worked with these products in production. The notes show the vocabulary, the support tasks and how I would learn the local system.

## 1. The big picture

Large organizations run several connected systems. A person applying for a permit, for example, touches a public web portal, a permitting application, a payment system, document storage, reports and maps.

| Layer | Purpose | Example in general terms |
| --- | --- | --- |
| Transaction systems | Record daily work | Permitting and land management, finance, HR, work orders |
| Enterprise resource planning (ERP) | Integrated finance, procurement, HR and assets | SAP is a widely used ERP suite |
| Document management | Store plans and records | Imaging and records systems |
| Data warehouse | Combined copy of data for reporting | Nightly loads from the transaction systems |
| Reporting and analytics | Dashboards and reports | SAP BusinessObjects, Crystal Reports, Power BI |
| GIS | Maps and spatial data | Parcel, zoning and inspection maps |
| Integration | Move data between systems | Interfaces, APIs, scheduled jobs |

## 2. ERP systems such as SAP (concepts)

An ERP system stores a company's core business data in one integrated database, split into modules (finance, purchasing, assets, HR, materials).

Terms: **client** (a data partition), **user role and authorization** (what a person may do), **transaction code** (a shortcut to a screen), **master data** (vendors, materials, employees) and **batch job** (a scheduled background task).

What first-line support usually does:

- Confirm the user's network, VPN and desktop client or browser access work.
- Check that the correct client software or browser version, and the shortcut or portal address, are installed.
- Reset or unlock accounts when authorized, but send role and authorization changes to the system's security administrator.
- Collect the exact error text, screenshot, transaction or screen name, time and user for the application team.
- Never share a login or bypass an approval workflow.

## 3. Permitting and land-management systems

Development services departments use specialized software to track applications, plans, fees, inspections and approvals through each step of a process. Typical support issues are browser compatibility, printing reports and plans, document upload limits, mobile inspection tablets that cannot sync, slow screens over a poor connection, and user access.

How I would learn such a system: take the vendor or department training, shadow users, sit through the process from application to inspection, write the common questions into knowledge base articles and learn who owns each layer (vendor, City IT, department).

## 4. Data warehouses and queries

A **data warehouse** stores data copied from operational systems, organized for reporting. It is refreshed on a schedule (often nightly), so a report may not show this morning's changes.

| Term | Meaning |
| --- | --- |
| ETL | Extract, transform, load: the process that fills the warehouse |
| Fact and dimension | Measures (permit counts, fees) and descriptive categories (date, area, type) |
| Data mart | A smaller warehouse for one department |
| Refresh | The time data was last loaded |
| Data quality | Whether values are complete, consistent and current |

When a report looks wrong, check: the filters and date range, the data refresh time, whether the user has permission to see all rows, and whether the source system was updated. Report the report name, parameters, expected and actual values and the time.

## 5. Reporting tools: BusinessObjects and Crystal Reports (concepts)

Reporting platforms let people run, schedule and share formatted reports against a database or warehouse.

- **Crystal Reports** designs pixel-perfect printed reports with grouping, formulas and subreports.
- **SAP BusinessObjects** is a platform that includes Web Intelligence for ad hoc queries, dashboards and a portal where reports are published, scheduled and secured.
- **A universe or semantic layer** gives business names to database fields so users can build queries without writing SQL.

Common support issues: login or single sign-on failures, a report prompting for parameters that the user does not understand, timeouts on large reports, an export to PDF or Excel failing, a scheduled report that did not run, and a missing report folder permission. Send design and data problems to the report owner.

## 6. GIS: geographic information systems (concepts)

GIS stores and displays information by location. Data is organized in **layers** (parcels, streets, zoning, flood zones, inspection locations), each with a table of attributes.

| Term | Meaning |
| --- | --- |
| Vector data | Points, lines and polygons (a parcel is a polygon) |
| Raster data | Grids of cells such as aerial imagery |
| Coordinate system or projection | How the round Earth is flattened onto a map. Layers with different systems will not line up |
| Geocoding | Turning an address into a map location |
| Attribute table | The data table behind each layer |
| Web map and map service | A published map that browsers and apps display |

Support tasks include confirming the user can open the web map, checking the browser and permissions, clearing the browser cache, helping with printing a map to PDF and noticing when a layer is missing or out of date. Map data edits, projection problems and service outages go to GIS staff.

## 7. Government software considerations

- **Records and privacy:** treat applicant and property information as sensitive. Share only what the ticket requires, and follow public-records and retention rules.
- **Accessibility:** public portals should be usable with keyboard and screen readers.
- **Change control:** systems used by the public are changed only through approved processes and maintenance windows.
- **Vendor coordination:** know the vendor support process, the contract contact and the information vendors ask for (version, steps, screenshots, log files).
- **City-wide IT:** departments often rely on a central IT department for networks, accounts, imaging and security. Know what stays in the department and what goes to central IT.

## 8. Troubleshooting template for a business application

1. Who is affected (one user, a group, everyone)? Since when?
2. What is the exact task and error text? Collect a screenshot.
3. Does it work in another browser, on another computer or from another user?
4. Is the network, VPN and sign-in working?
5. Was there a recent change or maintenance?
6. Check the application's status page or announcement if one exists.
7. Escalate to the application owner with the user, time, steps, error, scope and impact.

## 9. How I would learn the local systems

| Step | Action |
| --- | --- |
| Week 1 | Learn which systems the department uses, who owns each and the support paths |
| Week 2 to 4 | Complete user training, shadow staff and note repeated questions |
| Month 2 | Write knowledge base articles for the top issues and test them with users |
| Ongoing | Keep a glossary of terms and a diagram of how the systems connect |

## 10. Study questions

1. What is the difference between a transaction system and a data warehouse?
2. A report shows yesterday's permits but not today's. What is the likely reason?
3. Why do two map layers sometimes not line up?
4. A user can sign in to the network but not to the ERP. What do you check before escalating?
5. Who should change a user's role in an ERP system?
