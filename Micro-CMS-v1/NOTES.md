# Micro-CMS v1

## Summary
**Source:** Hacker101 CTF
**Category:** Web / HTML sanitization / Page creation
**Total Flags:** 4 (Flag0, Flag1, Flag2, Flag3)
**Points:** ?

---

## Hints Summary

### Flag0 - Page Discovery
- Try creating a new page
- How are pages indexed?
- Look at the sequence of IDs
- If the front door doesn't open, try the window
- In what ways can you retrieve page contents?

### Flag1 - Input Validation
- Make sure you tamper with every input
- Have you tested for the usual culprits? XSS, SQL injection, path injection
- Bugs often occur when an input should always be one type and turns out to be another
- Remember, form submissions aren't the only inputs that come from browsers

### Flag2 - Indirect XSS
- Sometimes a given input will affect more than one page
- The bug you are looking for doesn't exist in the most obvious place this input is shown

### Flag3 - Non-Script HTML
- Script tags are great, but what other options do you have?
- You have all the hints for this level.

---

## Recon
- The edit page shows a form with `title` and `body` and notes "Markdown is supported, but scripts are not".
- The body allowed markdown and some HTML (images rendered).
- The edit link was relative: `edit/2` and the resolved form action was `/page/edit/2`.

---

## Vulnerability theme
- Application allowed HTML/Markdown that could be used to cause the browser to request internal endpoints (e.g. `/flag`).
- Payloads such as markdown image/link or iframe/object (if rendered) can be used to fetch hidden resources.

---

## Steps to reproduce / Exploit path
1. Start the challenge instance and open edit link.
2. POST a markdown payload to the form action (use browser form submission to avoid CORS/preflight issues).
3. Create a markdown link or image referencing candidate endpoints (e.g. `/flag`, `/pages/2`).
4. Open the view page and follow the link or inspect embedded content to reveal the flag.

---

## Final payloads / commands
```html
<!-- Inject via edit form (markdown) -->
[open-flag](/flag)
![/flag](/flag)
```

```javascript
// Example script to POST from browser console (runs with cookies)
fetch('/page/edit/2', {
  method: 'POST',
  headers: {'Content-Type': 'application/x-www-form-urlencoded'},
  body: 'title=' + encodeURIComponent('ctf') + '&body=' + encodeURIComponent('[open-flag](/flag)'),
  credentials: 'include'
})
```

---

## Flags Captured

### Flag0 - Page Discovery
**Status:** Not found
**Flag:**

### Flag1 - Input Validation
**Status:** Not found
**Flag:**

### Flag2 - Indirect XSS
**Status:** Not found
**Flag:**

### Flag3 - Non-Script HTML (Markdown Links/Images)
**Status:** Found
**Flag:** (paste flag here)
**Method:** Used markdown link/image syntax to access `/flag` endpoint

---

## Artifacts
- artifacts/screenshots/
- artifacts/requests.txt

---

## Lessons learned
- When HTML is allowed, try non-script elements (markdown links, images, iframe/object) to access internal endpoints.
- Prefer normal form submission when fetch/XHR returns status 0 due to server quirks.
