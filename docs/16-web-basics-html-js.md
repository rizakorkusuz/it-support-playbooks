# Web Basics for Support: HTML, JavaScript and XML

Support technicians often meet web problems: a form that does nothing, a page that looks wrong, a data feed that will not load. You do not need to be a developer to find the broken piece and hand a good report to the right person. This guide covers the basics.

## 1. How a web page is built

| Part | Job | Example |
| --- | --- | --- |
| HTML | Structure and content | Headings, paragraphs, links, forms |
| CSS | Appearance | Colors, layout, fonts |
| JavaScript | Behavior | Validation, buttons, loading data |
| XML or JSON | Data exchange | Feeds, configuration, API responses |

## 2. A minimal page

```html
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <title>Help desk</title>
</head>
<body>
  <h1>Request help</h1>
  <form id="f">
    <label for="who">Your name</label>
    <input id="who" name="who">
    <button type="submit">Send</button>
  </form>
  <script>
    document.getElementById('f').addEventListener('submit', function (e) {
      e.preventDefault();
      console.log('Submitted by', document.getElementById('who').value);
    });
  </script>
</body>
</html>
```

## 3. Browser developer tools

Press **F12** (or Ctrl+Shift+I). The most useful tabs:

- **Elements**: the live HTML and CSS. Right-click an item and choose Inspect.
- **Console**: JavaScript errors and messages. Red lines are the place to start.
- **Network**: every request, with status codes and timing.
- **Application / Storage**: cookies and local storage.

## 4. Troubleshooting a page

| Symptom | Where to look | Typical cause |
| --- | --- | --- |
| Page blank or partial | Console and Network | A script failed to load, or a request returned 404 or 500 |
| Button does nothing | Console | JavaScript error, wrong element ID, event handler not attached |
| Form will not submit | Elements and Console | Required field missing, validation script error, wrong form action |
| Wrong layout or missing image | Elements and Network | CSS file not loaded, wrong path, case-sensitive file name |
| Works in one browser only | Console in both | Unsupported feature, extension, old browser or compatibility mode |
| Works for one user only | Account and cache | Permissions, stale cache (hard refresh with Ctrl+F5), extension |
| Mixed content or certificate warning | Console | HTTP resource on an HTTPS page, expired certificate |

## 5. HTTP status codes

| Code | Meaning | Support action |
| --- | --- | --- |
| 200 | OK | The request worked |
| 301 or 302 | Redirect | Check the destination |
| 400 | Bad request | Input or request is malformed |
| 401 or 403 | Not signed in or not allowed | Check sign-in and permissions |
| 404 | Not found | Wrong URL, or the page was removed |
| 500 | Server error | Report to the application owner with time and URL |
| 503 | Service unavailable | Server overloaded or down: check status and escalate |

## 6. JavaScript basics worth knowing

```javascript
const name = 'Pat';                         // a value
function greet(n) { return 'Hello ' + n; }  // a function
const items = [1, 2, 3].map(x => x * 2);    // [2, 4, 6]
document.querySelector('#out').textContent = greet(name); // change the page
fetch('/data.json').then(r => r.json()).then(console.log); // load data
```

Common mistakes: a typo in an element ID, a missing closing bracket or quote, running a script before the page has loaded, and comparing with `=` instead of `===`.

## 7. XML basics

XML stores data in nested tags. It must be well-formed: one root element, every opened tag closed, correct nesting, quoted attributes and escaped special characters (`&amp;`, `&lt;`, `&gt;`).

```xml
<?xml version="1.0" encoding="UTF-8"?>
<tickets>
  <ticket id="INC-0001" priority="P3">
    <summary>Printer offline</summary>
  </ticket>
</tickets>
```

Check well-formedness with a browser (it shows a parse error), an editor, or `xmllint --noout file.xml`. Validating against a schema (XSD) is a separate step.

## 8. Security basics for support

- Never paste passwords or tokens into forms you did not expect, or into public forums.
- Be careful with browser extensions and unknown scripts.
- Do not disable certificate warnings to make something work. Report them.
- Form data should travel over HTTPS only.

## 9. Writing a good web bug report

Include the URL, browser and version, steps to reproduce, expected and actual behavior, the exact error from the Console, a screenshot, the time, and whether it fails for other users or browsers.

## 10. Practice project

The `it-helpdesk-web-forms` repository contains a small HTML and JavaScript ticket-intake form with tests. Open it, break it on purpose (rename an element ID), and use the browser Console to find the error.
