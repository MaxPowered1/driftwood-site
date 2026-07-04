# Driftwood Apps — website

Static marketing site for Driftwood Apps and DriveTwin. No build step, no dependencies —
plain HTML + one shared stylesheet.

```
index.html            Studio homepage
drivetwin/index.html  DriveTwin product page
styles.css            Shared styles (light + dark via prefers-color-scheme)
netlify.toml          Netlify config (publish root, security headers)
```

## Deploy (GitHub → Netlify)

1. **Create the GitHub repo** (from this folder):
   ```bash
   cd ~/Desktop/driftwood-site
   git init && git add -A && git commit -m "Initial site"
   gh repo create driftwood-site --public --source=. --push
   ```
2. **Connect Netlify:** [app.netlify.com](https://app.netlify.com) → Add new site →
   Import an existing project → pick the `driftwood-site` repo. Build command: none.
   Publish directory: `.` (netlify.toml already says so). Deploy.
3. **Custom domain:** Netlify → Domain management → Add a domain you already own →
   `driftwoodapps.com`. Then add DNS records at the registrar (see gotchas below).
4. Every future `git push` redeploys automatically.

## DNS gotchas (learned the hard way on Porkbun — live config)

The site is served from Netlify; DNS stays at Porkbun so email forwarding keeps working.

- **Records at Porkbun (external DNS — do NOT switch nameservers to Netlify, it breaks email):**
  - `A` @ root → `75.2.60.5` (Netlify)
  - `CNAME` @ `www` → `sensational-cassata-ebac4e.netlify.app`
  - Leave MX records alone (they carry `hello@`/`rob@` forwarding to Gmail).
- **Kill Porkbun's default URL forwarding first.** New Porkbun domains ship with URL
  forwarding ON, redirecting the bare domain to a Link-in-Bio parking page. It synthesizes
  the root A record and **overrides** the one you add. It is NOT shown in the DNS Records
  list — remove it via Account → Domain Management → domain "Details" → URL Forwarding →
  trash the entry. Removing it also clears the root A record, so re-add `A → 75.2.60.5` after.
- **Porkbun DNS is Cloudflare-backed** → root A took ~18 min to propagate even at the
  authoritative nameserver. Be patient; poll `dig @fortaleza.ns.porkbun.com driftwoodapps.com A`.
- Full details + verification commands: see the "Distribution & Website" note in the
  project's Obsidian vault.

## Before launch — replace the placeholders

- [ ] **Buy links:** two `href="#"` links marked `TODO` (hero + pricing box on the
      DriveTwin page) → replace with the Gumroad / Lemon Squeezy product URL.
- [ ] **Screenshots:** two `.shot` placeholder divs (studio homepage product card +
      DriveTwin verification section) → replace with real app screenshots (`<img>`).
- [ ] **Email:** `hello@driftwoodapps.com` appears in nav/footers → set up forwarding
      at the registrar (or swap for a real address).
- [ ] **Names:** "Driftwood Apps" / "DriveTwin" are proposals — global find-and-replace
      if renamed. Check trademark conflicts before buying domains.
- [ ] **App icon → favicon** once the app has an icon.

## Conventions

- Accent gradient: `--accent` (#0071e3) → `--accent-2` (#00c7be), defined in styles.css.
- New product pages: copy `drivetwin/` as a template, add a product card to `index.html`.
- Dark mode is automatic (`prefers-color-scheme`) — check both when editing styles.
