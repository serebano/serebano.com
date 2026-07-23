# Project notes — serebano.com

A record of what was built, how, and the key decisions. Maintained alongside the site.

## What this is

A personal + company website for **Sergiu Toderascu** (`@serebano` on GitHub). It also serves as the
**public company website for the registered legal entity SERGIU TODERASCU, AI** (D-U-N-S® 933919838),
published openly for verification — including the Apple Developer Program individual → company
account conversion, which requires a matching live company website and D-U-N-S® number.

## Tech

- A single, dependency-free static `index.html`. No build step, no frameworks, no JS dependencies.
- Inline CSS + small inline scripts: theme toggle (`localStorage` + `data-theme`), year stamp,
  IntersectionObserver scroll reveal (respects `prefers-reduced-motion`).
- Dark-by-default with light-mode via `prefers-color-scheme`, plus a manual system → light → dark cycle.
- Open Graph / Twitter cards + `og.png` (1200×630), JSON-LD (Person / Organization / WebSite).
- Print stylesheet for verification printouts.
- Custom SVG favicon embedded as a data URI.
- Ops files: `robots.txt`, `.well-known/security.txt` (+ root `security.txt`).
- Why static + dependency-free: maximum reliability and zero maintenance. Apple's verification needs a
  stable, always-up company website; a single file with no build step can't break and deploys anywhere.

## Repo

- **Public repo:** https://github.com/serebano/serebano.com
- Branch: `main`. Pushed via the `gh` CLI authenticated as `serebano`.
- Files: `index.html`, `og.png`, `CNAME` (→ `serebano.com`), `robots.txt`, `security.txt`,
  `.well-known/security.txt`, `README.md`, `NOTES.md`, `.gitignore`.

## Hosting: GitHub Pages

Enabled via the GitHub API (not the UI):

```sh
gh api --method POST repos/serebano/serebano.com/pages \
  -f "source[branch]=main" -f "source[path]=/"
```

Pages serves from `main` / root. Custom domain `serebano.com` is set via the `CNAME` file and attached
to the Pages config.

## DNS setup

Domain registrar: Namecheap (NS: `dns1/dns2.registrar-servers.com`). Records added at the registrar:

| Host | Type | Value |
|------|------|-------|
| `@`  | A    | `185.199.108.153` |
| `@`  | A    | `185.199.109.153` |
| `@`  | A    | `185.199.110.153` |
| `@`  | A    | `185.199.111.153` |
| `www`| CNAME| `serebano.github.io.` |

Notes:
- These are GitHub Pages' official IPs. Apex uses A records (not CNAME).
- If the domain were on Cloudflare, the A records must be DNS-only (grey cloud), not proxied — GitHub
  must serve the TLS cert directly.

## HTTPS provisioning

This was the only non-instant step. Sequence and what to expect next time:

1. After DNS propagated, the site came up over **HTTP** immediately (`http://serebano.com` → HTTP 200).
2. GitHub Pages runs a **"DNS Check in Progress"** before requesting a Let's Encrypt cert. During this
   window, `https://serebano.com` served GitHub's generic `CN=*.github.io` wildcard cert → browsers
   flagged it as invalid (hostname mismatch). This is the normal transient state, not a config error.
3. The GitHub API returned `"certificate does not exist yet"` when attempting to enable enforcement
   (`PUT .../pages -F https_enforced=true`) until the cert was issued.
4. The cert provisioned ~30 min after DNS resolved. Enforcement was then enabled via:

   ```sh
   gh api --method PUT repos/serebano/serebano.com/pages -F "https_enforced=true"
   ```

5. Final state: `CN=serebano.com` (Let's Encrypt), HTTP/2 200, HTTP → HTTPS redirect active.

Diagnostics run during the wait that ruled out common causes (all clean):
- `dig serebano.com CAA` → no CAA restrictions blocking Let's Encrypt
- `dig serebano.com A` → correct GitHub Pages IPs
- `dig serebano.com AAAA` → none (no IPv6 conflict)
- `http://serebano.com/.well-known/acme-challenge/` → served by `GitHub.com` (ACME HTTP-01 path works)
- GitHub status → all systems operational

If a future cert gets stuck (>1 hr), the reliable nudge is to remove the custom domain in
**repo Settings → Pages** and re-add it — that re-triggers the DNS check + cert request.

## Content / decisions

The site content is derived from the public GitHub profile at github.com/serebano.

Direction taken (and revised) over the session:
- **Focus:** developer tooling + AI engineering. **Busymate DevTools** (busymate.dev) is spotlighted in
  the Work section with its capabilities (capture across iOS/Android/browser/device farm, inspect &
  manipulate, BusyBro AI teammate, 303-tool MCP server, enterprise security).
- **About** is written generically/professionally, centered on devtooling + AI engineering rather than a
  personal narrative.
- **BusyBrothers** is referred to as a **team** (not "studio" / "organization").
- **Removed:** the open-source repository listing (no repo index), the Twitter handle, and the
  Apple-verification explainer paragraph from the Company section.
- **Contact:** `hi@serebano.com` — MX points at Namecheap Email Forwarding
  (`eforward*.registrar-servers.com`) with matching SPF; confirm the forward target in the Namecheap
  panel if delivery needs checking.
- **Company & legal entity** section keeps the full SERGIU TODERASCU, AI block (legal name, D-U-N-S®,
  address, city, postcode, country) plus a footer repeat, so the verification info appears site-wide.
  Subline clarifies sole proprietorship / individual enterprise (Moldova).

## Editing the site

Just edit `index.html` — everything (markup, styles, script) is in that one file. Then:

```sh
git add -A
git commit -m "describe change"
git push origin main
```

GitHub Pages auto-rebuilds from `main` on push; changes are live at https://serebano.com within a minute.