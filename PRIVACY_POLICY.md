# Privacy Policy for SiteGuard — Local Security Advisor

**Effective date:** November 9, 2026 (version 1.1.0)

SiteGuard is a browser extension that helps you assess the security characteristics of websites you visit. It works **locally, inside your browser**. The developer does not operate any server that SiteGuard sends data to, and **the developer does not receive, collect, or have access to any data about you or your browsing.**

This policy explains what SiteGuard processes, where it is stored, when (and only when) anything leaves your device, and what choices you have.

## 1. Information SiteGuard processes

When you visit a web page (http or https), SiteGuard analyzes security-relevant information about that page, **on your device**, to calculate a 0–100 risk score and a list of findings. This can include:

- The website's address and characteristics of its domain name (for example, whether it looks like a well-known brand's domain)
- Whether the page uses HTTPS, and HTTP security response headers (such as Content-Security-Policy and Strict-Transport-Security)
- The **names and security attributes** of cookies the site sets (Secure, HttpOnly, SameSite). SiteGuard **never reads or stores cookie values.**
- Redirects the page went through before loading
- Page structure signals: whether there are password forms and where they submit, mixed-content resources, third-party script origins, hidden frames, scripts that look obfuscated, links to sensitive-looking file paths, and whether the page displays well-known server error messages. SiteGuard records only small structured results (for example "a password form submits over HTTP"), **not the page's text or HTML.**

This processing is the extension's single purpose: website security assessment. It is a user-facing feature described in the extension's listing and interface.

## 2. Information stored on your device

SiteGuard stores the following using the browser's extension storage, on your device only:

- **Your settings**: scanning mode, sensitivity, history-retention period, and your allow list and block list of domains
- **Scan history**: for each scan, the page address **without its query string or fragment** (query strings can contain tokens or personal data, so they are removed), the risk score, the findings, and a short explanation
- **False-positive marks** you create with the "Mark as false positive" button (these stay on your device and are not sent anywhere)
- **Optional backend settings** (see section 3): the server URL, your consent choice, and, if you enter one, the API token

SiteGuard also keeps short-lived, in-memory working data for open tabs (for example the result of the latest scan, so the toolbar popup can show it). This is cleared when you close the tab or the browser. Raw headers and page signals are discarded as soon as a scan finishes.

SiteGuard does **not** store: passwords, authentication credentials, payment card information, personal communications, cookie values, complete web-page HTML, keystrokes, or mouse movements.

## 3. Optional Security Analysis backend (off by default)

SiteGuard works fully without any backend. Advanced users can optionally connect SiteGuard to a Security Analysis API **they choose** in Settings. **Nothing is sent unless you both (a) enter a backend URL and (b) tick the consent box in Settings.** Changing the server resets your consent.

If you do this, then for each scanned page SiteGuard sends the following to **the server you entered** (over HTTPS; plain HTTP is only accepted for `localhost`):

- The site's **domain name** (for example `example.com`) and whether it uses HTTPS
- A few yes/no or count signals: whether HSTS and CSP headers are present, the number of mixed-content resources, redirect counts, the number of third-party script origins, and whether the page has a password field
- If you entered one, your API token, in an `Authorization` header

SiteGuard does **not** send full URLs, paths, query strings, page content, cookie values, or header values. **Domains on private or internal networks** (such as `localhost`, `.local`, or private IP addresses) are **never** sent.

Responses from the backend are treated strictly as data: they are validated and displayed as findings, and are never executed as code. If the backend is unreachable, SiteGuard tells you and continues with local results only.

The backend is operated by whoever you configured, not by the SiteGuard developer. Review that operator's privacy policy before enabling it. You can withdraw consent or clear the URL at any time.

## 4. Why information is used

Information is used only to: detect observable website security issues; generate the risk score and findings; show warnings, the toolbar badge and reports; enforce your allow and block lists; keep your local scan history; and, only if you opted in, request additional analysis from your chosen backend.

SiteGuard does not use information for advertising, behavioral profiling, credit or lending decisions, or any purpose unrelated to its security-assessment function. The use of information received by SiteGuard adheres to the Chrome Web Store User Data Policy, including its Limited Use requirements.

## 5. Sharing

SiteGuard does not sell user data and does not transfer it to third parties for advertising or unrelated purposes. By default, **no data is shared with anyone.** The only possible recipient is the backend server you voluntarily configure and consent to, as described in section 3. SiteGuard contains no analytics, tracking, or crash-reporting code.

## 6. Browsing activity

To work, SiteGuard observes the pages you load, because it must read each page's response headers and structure to score it. In automatic mode this happens for every http/https page you visit; in manual mode SiteGuard only analyzes a page when you open the popup or press Rescan. This information stays on your device (see section 2) except as described in section 3. SiteGuard does not create a browsing profile, and the developer cannot see it.

## 7. Cookies and website content

SiteGuard reads only the *attributes* of cookies sent by a site (to check the Secure, HttpOnly and SameSite flags). It never reads or stores their values. It inspects technical features of a web page to produce security findings and does not collect page content for any other purpose.

## 8. Permissions

SiteGuard requests only the permissions its features need:

| Permission | Why it is needed |
|---|---|
| Access to http/https websites | To run the security scan on pages you visit: read response headers and examine page structure, and to show SiteGuard's own warning on the page |
| `webRequest` (read-only) | To read response headers and follow redirect chains. SiteGuard never modifies requests |
| `declarativeNetRequest` | To block domains on **your** block list before they load |
| `storage` | To save your settings and local scan history on your device |

SiteGuard does **not** request the `history`, `cookies`, `tabs`, `webNavigation`, `alarms`, or `activeTab` permissions.

## 9. Retention and deletion

- **Scan history** is kept for the period you choose (7 days, 30 days — the default, 90 days, or 1 year). Expired entries for **all** sites are deleted automatically when the browser starts, when you change the retention period, and during normal scanning (at most once a day). SiteGuard also caps how much history it keeps per site and in total.
- You can delete all stored scan history at any time with **Settings → Delete all local scan history**.
- Removing the extension deletes all of its stored data from your browser.
- Data sent to an optional backend is subject to that operator's retention practices.

## 10. Your controls

In Settings you can: choose automatic or manual scanning; set sensitivity; manage allow and block lists; set or disable history retention; delete history; and leave the backend blank (the default) or enable it with explicit consent.

## 11. Security

SiteGuard processes data locally to reduce unnecessary transmission. Any backend connection must use HTTPS. No method of electronic storage or transmission is perfectly secure, so please review any external service before enabling it.

## 12. Children's privacy

SiteGuard is not directed at children and does not knowingly collect personal information from children.

## 13. Changes to this policy

If SiteGuard's data practices change, this policy will be updated at the same address and the effective date above will change.

**Version 1.1.0 changes:** removed unused permissions; saved scan history no longer includes URL query strings or fragments (existing history is cleaned automatically on update); the optional backend now requires an explicit consent step and never receives private-network hostnames; expired history is now deleted for all sites, not only the site being scanned.

## 14. Contact

Developer: Dr. Hashmat Fida
Email: hashmatfida [at] gmail dot com
