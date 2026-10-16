# HTML, DHTML and XML Basics

This guide goes deeper than the Web Basics guide: how to read and hand-write HTML, what "DHTML" means, how to troubleshoot page behavior and how to read and validate XML. It is aimed at technicians who need to open a page's source and find what is broken, and who may make small fixes to simple pages.

## 1. HTML structure

HTML describes the structure of a page with elements written as tags. Use semantic elements so that browsers and assistive technology understand the page.

```html
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>Equipment request</title>
  <link rel="stylesheet" href="style.css">
</head>
<body>
  <header><h1>Equipment request</h1></header>
  <main>
    <p>Use this form to request a new device.</p>
    <table>
      <thead><tr><th>Item</th><th>Status</th></tr></thead>
      <tbody><tr><td>Laptop</td><td>Approved</td></tr></tbody>
    </table>
    <p><a href="/help/">Help</a></p>
  </main>
</body>
</html>
```

Key rules: close every element, nest properly, quote attribute values, give every image an `alt` attribute and every form field a `<label>`, and use `id` values only once per page.

## 2. Forms

```html
<form action="/submit" method="post">
  <label for="dept">Department</label>
  <select id="dept" name="dept" required>
    <option value="">Choose</option>
    <option>Permits</option>
    <option>Inspections</option>
  </select>
  <label for="note">Details</label>
  <textarea id="note" name="note" rows="4"></textarea>
  <button type="submit">Send</button>
</form>
```

The `name` attribute is the key sent to the server. `required` and `type="email"` add built-in validation, which should also be repeated on the server.

## 3. CSS in a nutshell

CSS selects elements and sets how they look. Specificity decides which rule wins: inline styles beat IDs, which beat classes, which beat element selectors. Use the browser's Elements panel to see which rules apply and which are overridden.

## 4. What DHTML means

"DHTML" (Dynamic HTML) is an older term for pages that change without reloading by combining HTML, CSS and JavaScript through the **Document Object Model (DOM)**. Today it is simply how modern web pages work. The ideas are the same:

| Part | Role |
| --- | --- |
| HTML | The content |
| CSS | Presentation, including show and hide and animations |
| JavaScript | Reacts to events and changes the DOM |
| DOM | The live tree of the page that scripts read and change |

```html
<button id="toggle">Show details</button>
<div id="details" hidden>More information.</div>
<script>
  var btn = document.getElementById('toggle');
  btn.addEventListener('click', function () {
    var box = document.getElementById('details');
    box.hidden = !box.hidden;
    btn.textContent = box.hidden ? 'Show details' : 'Hide details';
  });
</script>
```

Older DHTML code used browser-specific objects (such as `document.all` and `layer`). When you meet legacy pages that depend on them, expect compatibility mode or modern-browser problems and escalate to the page owner.

## 5. Troubleshooting HTML and DHTML

1. **Reproduce** the problem and note the browser and version.
2. Open **Developer tools** (F12). Look at the Console for errors, the Elements panel for the live DOM and styles, and the Network panel for failed requests.
3. **Validate** the markup with a validator (for example the W3C Nu HTML Checker). Unclosed tags and duplicate IDs cause strange behavior.
4. **Check paths**: a missing image or stylesheet is usually a wrong path or capitalization on a case-sensitive server.
5. **Check scripts**: a syntax error stops the whole script. `undefined` or `null` errors usually mean an element ID does not exist yet or was mistyped.
6. **Clear cache** (hard refresh with Ctrl+F5) to be sure you are testing the latest files.
7. **Compare browsers.** If only one fails, look for unsupported features, extensions or legacy compatibility settings.
8. **Change one thing at a time** and retest. Keep a copy of the original file.

| Symptom | Likely cause |
| --- | --- |
| Text appears as garbled characters | Missing `<meta charset="utf-8">` or wrong file encoding |
| Layout breaks on phones | Missing viewport meta tag, fixed widths |
| Click does nothing | JavaScript error, wrong ID, element covered by another |
| Form submits to the wrong place | Wrong `action` or `method` |
| Table is misaligned | Missing `</td>` or `</tr>`, CSS borders |
| Mixed content warning | An `http://` file loaded on an `https://` page |

## 6. XML basics

XML (Extensible Markup Language) stores structured data in tags you define. It is used for configuration files, data exchange and feeds such as RSS and sitemaps.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<inventory updated="2026-10-15">
  <device tag="AST-0001" status="Assigned">
    <model>Laptop L14</model>
    <location>Floor 2</location>
  </device>
  <device tag="AST-0002" status="In stock">
    <model>Desktop D7</model>
    <location>Storage A</location>
  </device>
</inventory>
```

**Well-formed** means: one root element, every tag closed, correct nesting, attribute values quoted, case-sensitive names, and special characters escaped (`&amp;`, `&lt;`, `&gt;`, `&quot;`, `&apos;`).

**Valid** means the file also follows a schema (XSD or DTD) that defines allowed elements, their order and data types.

### Checking XML

- Open the file in a browser or editor. A parse error shows the line and column.
- Command line: `xmllint --noout file.xml` (well-formed), `xmllint --noout --schema rules.xsd file.xml` (valid).
- PowerShell: `[xml](Get-Content file.xml -Raw)` throws an error with a position if the XML is not well-formed, and lets you read values: `$x.inventory.device | Select tag, status`.

### Common XML problems

| Error | Cause |
| --- | --- |
| "Opening and ending tag mismatch" | A missing or misspelled closing tag |
| "Premature end of data" | File cut off by a failed download or save |
| "Invalid character" | Unescaped `&` or `<`, or a wrong encoding |
| "Extra content at the end" | More than one root element |
| Schema validation errors | A required element is missing or a value has the wrong type |

## 7. XML, JSON and HTML compared

| Format | Best for | Notes |
| --- | --- | --- |
| HTML | Human-readable pages | Browsers are forgiving of errors, which hides mistakes |
| XML | Documents and configuration with strict structure, schemas | Verbose but self-describing |
| JSON | Data exchange with web services | Shorter, no comments, strict syntax |

## 8. XSLT and transformation (concept)

XSLT is a language that transforms XML into other XML, HTML or text. Support staff mostly meet it inside existing systems. Know that a broken transform usually means the input XML changed shape.

## 9. Practice exercises

1. Break a page by removing a closing tag, and use the validator to find it.
2. Rename an element ID so a script stops working, then find the error in the Console.
3. Create an XML inventory with three devices and validate that it is well-formed.
4. Read the XML into PowerShell and list devices that are In stock.

The `it-helpdesk-web-forms` repository has a small HTML and JavaScript form with tests to practice on.

## 10. Scope note

Hand-coding and fixing simple pages is practical for support work. Complex scripts, legacy browser-specific code and production web applications belong to developers. Escalate with the URL, browser, steps to reproduce and the exact error.
