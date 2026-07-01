# my-little-uw-engine

DHCD Form 202 underwriting engine — browser-based, password-protected.

**Live app:** `https://rawtjgreen.github.io/my-little-uw-engine/`

---

## Access

The app requires a passphrase on every visit. Sessions persist until you close the browser tab or click Lock. After 5 failed attempts the gate locks for 10 minutes.

## What it does

Drop a completed, Excel-saved DHCD Form 202 (.xlsm or .xlsx) into the page. The engine:

- Tests 45 DHCD underwriting standards (DSCR, HPTF %, vacancy, PUPA expense cap, contingencies, escalations, reserves, developer fee, and more)
- Classifies each as **PASS / FAIL / REVIEW / DATA ISSUE**
- Shows Sources & Uses, financing gap, and debt sizing at a glance
- Detects manual calculation mode and warns you
- Lets you add waiver notes inline for FAIL and REVIEW items
- Downloads a scored workbook (.xlsx) or export (.csv) with your notes

**Nothing is transmitted.** Files are processed entirely in your browser.

## Files

| File | Purpose |
|------|---------|
| `index.html` | App — engine logic + UI + password gate |
| `criteria.json` | **Edit this** to update DHCD thresholds for a new round |
| `README.md` | This file |

## Updating criteria for a new year

Open `criteria.json` in GitHub, find the standard, update the `threshold`, commit. The app picks it up on next page load. Every change is version-controlled with a commit timestamp — full audit trail.

Example — updating the PUPA expense cap:
```json
{ "key": "pupa_expense", "threshold": 11200, ... }
```

## Deploy (first time)

1. Create repo `my-little-uw-engine` on GitHub (Public or Private)
2. Upload `index.html`, `criteria.json`, `README.md`
3. Settings → Pages → Branch: main → / (root) → Save
4. Live in ~60 seconds at `https://rawtjgreen.github.io/my-little-uw-engine/`

## Upgrade path: Private repo (Option B)

When you're ready to move beyond the client-side password gate to true access control:

1. Upgrade to GitHub Pro (~$4/mo)
2. Go to repo Settings → Change visibility → Private
3. GitHub Pages continues to serve the app, but only to GitHub accounts you invite as collaborators
4. Remove or keep the password gate — it becomes redundant but doesn't hurt

## Before each use

The Form 202 must be **opened and saved in Excel** with real deal data before uploading. The engine reads cached formula results. A 202 that hasn't been recalculated will show DATA ISSUE flags throughout — request a fresh save from the applicant.

## Security notes

- Passphrase stored as SHA-256 hash — the literal phrase is not in the source code
- 5-attempt lockout (10 minutes) against brute force
- Show/hide toggle on the passphrase field
- Lock button clears the session immediately
- Formula injection stripped from all cell values before display
- `textContent` used throughout — no `innerHTML` with external data
- Zero external network calls after initial page load
