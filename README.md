# SiteGuard — Local Security Advisor

A Manifest V3 Chrome extension that passively checks the websites you visit for HTTPS and security-header problems, insecure cookies, mixed content, suspicious redirects, look-alike domains and risky forms/scripts — **entirely on your device** — and gives each page a transparent 0–100 risk score with evidence-based reasons.

> SiteGuard is a *passive advisor*. It reports observable warning signs; it cannot prove a site is safe or vulnerable, and heuristic findings can be wrong. It is not antivirus.

**Current version: 1.1.0** · Privacy policy: [`PRIVACY_POLICY.md`](PRIVACY_POLICY.md)

## Repository layout

```
siteguard/        the extension (this folder is what gets zipped and uploaded)
tests/            automated tests + Web Store policy audit (not shipped)
PRIVACY_POLICY.md
SUBMISSION_NOTES.md
```

## Install for development

1. `chrome://extensions` → enable **Developer mode** → **Load unpacked** → select the `siteguard/` folder.
2. Visit any http/https page; the toolbar badge and popup show the result.

Chrome 112 or newer is required (`storage.session` quota).

## Permissions (and why)

| Permission | Purpose |
|---|---|
| `storage` | Settings and local scan history; short-lived per-tab results in `storage.session` |
| `webRequest` (read-only) | Read response headers and follow redirect chains — never modifies requests |
| `declarativeNetRequest` | Block domains on the user's own block list before they load |
| Host access `http://*/*`, `https://*/*` | Scan whichever site the user visits; run the content script that inspects page structure and shows warnings |

Deliberately **not** requested: `alarms`, `activeTab`, `tabs`, `webNavigation`, `history`, `cookies`, `scripting`, `downloads`. Retention pruning runs on browser startup, when the setting changes, and at most once a day during scans — no timers needed.

## How it works

```
webRequest (headers, redirects) ─┐
content script (page structure) ─┴─► rules.js ─► scoring.js ─► xai.js ─► badge · popup · warning · history
```

- `background.js` — service worker; per-tab state kept in `storage.session` so it survives worker restarts.
- `content.js` — collects small structured signals (no page text, no query strings) and renders warnings in a Shadow DOM.
- `lib/heuristics.js` — shared detection helpers · `lib/rules.js` — the checks · `lib/scoring.js` — decay-weighted score and the confidence/evidence gate that stops low-certainty findings from ever blocking · `lib/xai.js` — explanations that label each finding *detected fact* vs *heuristic inference* · `lib/storage.js` — settings, history, validation.

## Optional analysis backend (off by default)

Settings → *Optional backend*. Nothing is sent unless you enter an **https://** URL **and** tick the consent box. For each scanned page the server receives only: the domain name, the protocol, and a few yes/no/count signals (HSTS/CSP present, mixed-content count, redirect counts, third-party script count, password-field present) as `POST {url}/v1/scan/quick`. Never full URLs, page content or cookies; private/internal hostnames are never sent. Replies are validated and clamped, and treated purely as data.

Expected response: `{"top_findings":[{"id","category","title","severity","confidence","evidence_quality","evidence","remediation","cwe"}]}` (max 10 used).

## Settings

- **Mode** — Automatic, or Manual (scan only when you open the popup / press Rescan)
- **Sensitivity** — Strict (warn 25 / block 50), Balanced (35 / 60), Relaxed (45 / 75)
- **Allow / block lists** — apply to the domain and its subdomains; a domain can be on only one list
- **History retention** — 7 / 30 / 90 / 365 days, or delete everything with one click

## Tests

```
cd tests
npm install
npm test            # 70 tests + policy audit
node policy_audit.js /path/to/extracted/zip   # audit exactly what you will upload
SG_ROOT=/path/to/extracted/zip npm test        # run the suite against the packed files
```

The tests load the real extension code into a simulated browser (mocked `chrome` APIs, jsdom). They do **not** replace a manual run in real Chrome — see `SUBMISSION_NOTES.md` §6.

## Packaging

Zip the *contents* of `siteguard/` so that `manifest.json` is at the zip root (no `_metadata`, tests, or docs inside):

```
cd siteguard && zip -r -X -D ../siteguard-v1.1.0.zip manifest.json background.js content.js icons lib options popup report
```
