# Microsoft Access Basics for Support Staff

> **Study guide.** This is a concept guide for supporting and understanding small Access databases. It is not a claim of production experience building or administering Access systems.

Microsoft Access is a desktop database tool. Many departments still use small Access databases for tracking inventory, requests or simple reports, and support staff are asked to keep them opening, linked and backed up. This guide covers the vocabulary, common problems and good habits.

## 1. Core concepts

| Term | Meaning |
| --- | --- |
| Table | Stores records (rows) made of fields (columns). Each table is about one kind of thing |
| Primary key | A field that uniquely identifies each record, such as an asset tag |
| Relationship | A link between tables using a key, for example one user to many devices |
| Query | A saved question that selects, filters, sorts or calculates from tables |
| Form | A screen for entering and viewing records |
| Report | A formatted, printable view of query results |
| Macro and VBA | Automation inside the database. Treat existing code with care |
| Linked table | A table that lives in another file, database or server, but appears in this database |

## 2. Good structure

1. Keep one subject per table (devices, users, locations), not one wide spreadsheet.
2. Use a primary key for every table, and enforce **referential integrity** on relationships so records cannot be orphaned.
3. Choose correct data types (Date/Time, Number, Short Text, Yes/No) and set required fields and validation rules.
4. Split a shared database into a **front end** (forms, queries, reports) and a **back end** (tables). Each user has their own copy of the front end and all share one back end on a network location.

## 3. Queries

A select query shows data. SQL is the language behind it:

```sql
SELECT Devices.AssetTag, Devices.Model, Users.FullName
FROM Devices INNER JOIN Users ON Devices.UserID = Users.UserID
WHERE Devices.Status = 'Assigned'
ORDER BY Users.FullName;
```

Other query types change data (append, update, delete, make-table). Always back up before running an action query, and read the confirmation prompt.

## 4. Reports from a database

Base reports on queries, not directly on tables. Group and sort (for example devices by department), add totals, and test the report with real-sized data. Export to PDF or Excel for sharing.

## 5. Common support problems

| Symptom | Likely cause | Action |
| --- | --- | --- |
| "Database is read-only" or cannot save | Folder permissions, file marked read-only, file locked by another user | Check share and NTFS permissions, look for the `.laccdb` lock file, ask others to close |
| Security warning or macros disabled | Trust settings, file in an untrusted location | Open from a trusted location approved by policy. Do not lower security globally |
| "Could not find installable ISAM" or linked table errors | Back-end file moved or renamed, drive mapping changed | Use Linked Table Manager to relink |
| Very slow over the network | Wi-Fi, VPN, large unsplit file, queries without indexes | Use wired network, split the database, index key search fields |
| Database corrupted | Dropped connection while open, shared unsplit file | Close for everyone, copy the file, run **Compact and Repair**, restore from backup if needed |
| 32-bit versus 64-bit errors | VBA or drivers built for another Office bitness | Match the Office bitness or update declarations (a developer task) |
| Cannot connect to ODBC source | Missing driver or DSN, credentials | Check the data source in the ODBC administrator, escalate credentials |

## 6. Care and backup

- Back up the file before changes, and keep dated copies.
- Run **Compact and Repair** regularly to reduce size and fix minor issues.
- Do not open the same file from several people unless it is split.
- A single `.accdb` file is limited to 2 GB. Past a few users or a growing dataset, plan a move to a server database such as SQL Server.
- Protect sensitive data with file permissions. Access passwords are not strong security.

## 7. Access and Excel

Use Excel for quick analysis and Access when data has relationships, many records or several users. Access can import from and export to Excel and link to an Excel file. Keep one source of truth.

## 8. Escalate

Escalate to the database owner or a developer when VBA, macros or the design must change. Give the error text, the file location, the Access version and bitness, steps to reproduce and whether a backup exists.

## 9. Learning path

1. Build a small inventory database with Devices, Users and Locations tables.
2. Create a query that lists assigned devices, and a report grouped by department.
3. Split the database and relink the back end.
4. Practice Compact and Repair on a copy.
