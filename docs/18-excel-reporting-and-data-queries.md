# Excel Reporting and Data Queries

Support teams answer questions with numbers: how many tickets, which devices are out of warranty, what repeats. Excel is the most common tool for those reports. This guide covers reliable techniques and the concepts behind larger reporting tools.

## 1. Prepare the data

1. Keep data in a plain table: one header row, one record per row, no blank rows or merged cells.
2. Convert the range to a **Table** (Ctrl+T) so formulas, filters and charts grow with the data.
3. Use consistent values (the same status names and date formats). Clean text with `TRIM`, `PROPER` and Find and Replace.
4. Keep a copy of the raw data. Never overwrite the export.

## 2. Formulas worth knowing

| Task | Formula or feature |
| --- | --- |
| Count records | `=COUNTA(Table1[Ticket])` |
| Count with a condition | `=COUNTIFS(Table1[Status],"Open",Table1[Priority],"P1")` |
| Sum with a condition | `=SUMIFS(Table1[Cost],Table1[Dept],"Permits")` |
| Look up a value | `=XLOOKUP(A2,Assets[Tag],Assets[Owner],"Not found")` (or `VLOOKUP` in older versions) |
| Days open | `=TODAY()-[@Opened]` |
| Flag old items | `=IF([@Age]>30,"Review","")` |
| Remove duplicates | Data > Remove Duplicates, or `=UNIQUE()` in Microsoft 365 |
| Combine text | `=[@First]&" "&[@Last]` |

## 3. PivotTables

A PivotTable summarizes a table without formulas.

1. Click inside the Table and choose Insert > PivotTable.
2. Drag **Category** to Rows, **Priority** to Columns and **Ticket** to Values (set to Count).
3. Add a Slicer for Status or Department to filter quickly.
4. Refresh (Data > Refresh All) after the source changes.
5. Group dates by month or week to see trends.

## 4. Charts and presentation

Use a bar chart for categories, a line chart for trends over time and avoid 3-D charts. Label axes, use a clear title and keep colors consistent. Put the key number in the title, for example "Printing tickets doubled in October".

## 5. Importing and querying data

- **Get & Transform (Power Query):** Data > Get Data lets you import from CSV, other workbooks, databases and web sources, then clean the data with repeatable steps (remove columns, split text, change types, merge tables). Refresh to repeat the same steps on new data.
- **CSV exports** from ticketing and inventory tools are the usual starting point. Check encoding and the date format on import.
- **Linking to a database** (Access or SQL Server) uses ODBC or OLE DB connections. Use a read-only account for reporting.
- **Excel limits:** about 1,048,576 rows per sheet. For bigger data use a database or Power Query without loading everything to a sheet.

## 6. Report design checklist

- [ ] The question the report answers is stated at the top
- [ ] Data source and date range are written on the report
- [ ] Numbers can be traced back to the source rows
- [ ] Totals reconcile with the source
- [ ] Sensitive data is removed or protected before sharing
- [ ] The file is saved as PDF or a protected copy for distribution

## 7. Support-style reports to practice

| Report | Fields | Why it matters |
| --- | --- | --- |
| Tickets by category and priority | Category, priority, status | Where the effort goes |
| Open ticket aging | Opened date, age in days | SLA risk |
| Repeat issues | Category, summary | Candidates for knowledge base articles |
| Devices by status and age | Status, purchase date, warranty end | Refresh and budget planning |
| Warranty ending soon | Warranty end date | Avoid out-of-warranty repairs |

Sample CSV files and scripts that produce these summaries are in the `imac-asset-inventory-toolkit` repository.

## 8. Larger reporting tools (concepts)

Organizations that outgrow spreadsheets use reporting and business intelligence tools, for example SAP BusinessObjects (Crystal Reports, Web Intelligence), Power BI or SQL Server Reporting Services. They share the same ideas:

1. A **data source** (a database, data warehouse or file).
2. A **query or semantic layer** that defines what fields mean.
3. A **report** with filters, grouping and totals.
4. **Scheduling and distribution** so people receive refreshed reports.
5. **Security** that limits who sees which data.

A support technician's role is usually to help users open and run existing reports, check that the connection, login and permissions work, and escalate data or design problems to the report owner. This is a conceptual overview, not experience with those products.

## 9. Common Excel problems

| Symptom | Cause | Fix |
| --- | --- | --- |
| Dates appear as numbers | Cell format General | Format as Date, or fix the import type |
| Numbers stored as text | Leading apostrophe or imported text | Convert with Text to Columns or `VALUE()` |
| `#N/A` from a lookup | Value missing or spaces | Clean with `TRIM`, use the not-found argument |
| `#REF!` | Deleted referenced cells | Restore or rewrite the formula |
| File opens slowly | Large volatile formulas, many external links | Simplify, break links, save as `.xlsx` |
| Link warnings on opening | References to moved files | Data > Edit Links |
| Pivot does not update | Not refreshed or the range did not grow | Refresh, use a Table as the source |
