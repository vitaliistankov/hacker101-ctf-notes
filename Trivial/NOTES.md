# Trivial

## Summary
**Source:** Hacker101 CTF  
**Category:** Web / Steganography / File analysis  
**Points:** 1

---

## Recon
| Step | Action | Result | Tool |
|---|---|---|---|
| 1 | Open challenge page | "Welcome to level 0" | Browser |
| 2 | View source | Background image referenced in CSS | Browser |
| 3 | Open background.png | Downloaded file contained hidden flag via `strings` | curl / strings |

---

## Vulnerability theme
- Flag hidden inside static asset (image).
- Steganography/file analysis required.

---

## Steps to reproduce / Exploit path
1. Open page source and find `background-image: url("background.png")`.
2. Download `background.png`.
3. Run `strings background.png | egrep -i "flag|\^FLAG\^"` or use `exiftool`/`binwalk`.

---

## Final payloads / commands
```bash
curl -O https://ctf.hacker101.com/ctf/background.png
strings background.png | egrep -i "flag|\^FLAG\^"
```

---

## Flag
(paste flag here if desired)

---

## Artifacts
- artifacts/screenshots/
- artifacts/background.png

---

## Lessons learned
- Always inspect linked resources (images, JS) in minimal pages.
