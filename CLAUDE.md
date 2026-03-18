# CLAUDE.md — White Rabbit Public Website

## Project overview

This is the public-facing marketing website for **White Rabbit** — an AI-first custom software consultancy targeting LATAM. The site is written in plain HTML/CSS/JS (no framework, no build step). All content lives in the root of the repo.

**GitHub repo:** `whiterabbit-it/wr-site`

### Pages
| File | Purpose |
|---|---|
| `index.html` | Main landing page |
| `bruma.html` | Product page — Bruma |
| `vitacore.html` | Product page — VitaCore |

---

## Infrastructure & deployment

### Hosting: Firebase Hosting

The site is deployed to **Firebase Hosting** (free Spark plan). It provides CDN, HTTPS, and custom domain out of the box.

- **GCP / Firebase project:** `second-academy-418617` (White Rabbit)
- **Default URL:** `https://second-academy-418617.web.app`
- **Custom domain:** `whiterabbit.com.ar` (to be connected in Firebase Console)

### CI/CD: GitHub Actions + Workload Identity Federation

Push to `main` → GitHub Actions authenticates to GCP via WIF → deploys via Firebase CLI.

No JSON keys — authentication uses Workload Identity Federation:
- **WIF Pool:** `github-actions`
- **WIF Provider:** `github`
- **Service account:** `github-actions-deployer@second-academy-418617.iam.gserviceaccount.com`
- **WIF provider resource name:** `projects/646689989034/locations/global/workloadIdentityPools/github-actions/providers/github`

### Firebase config files
- `firebase.json` — hosting config (public dir, ignore list, cache headers, clean URLs)
- `.firebaserc` — project alias pointing to `second-academy-418617`

---

## Key operational notes

- **No build step.** Deploy the raw HTML files as-is.
- **Cache strategy:** HTML files are served with `Cache-Control: no-cache` (configured in `firebase.json`). Firebase CDN handles asset caching automatically.
- **`cleanUrls: true`** — `/bruma.html` is served at `/bruma` automatically.
- All pages are in **Spanish (es-AR)**.

### Connecting the custom domain
In Firebase Console → Hosting → Add custom domain → enter `whiterabbit.com.ar` → follow the DNS verification steps (adds a TXT record to Cloud DNS, then an A record).

---

## Local development

No local server needed. Open any `.html` file directly in a browser, or use:

```bash
python3 -m http.server 8080
```

Then visit `http://localhost:8080`. Or preview exactly as Firebase would serve it:

```bash
firebase serve --only hosting
```

---

## Design system (quick reference)

Defined in CSS custom properties inside each HTML file:

| Token | Value | Usage |
|---|---|---|
| `--dark` | `#0A0A0A` | Background |
| `--light` | `#F5F5F0` | Text / light surfaces |
| `--lime` | `#D4FF00` | Accent / CTA |
| Font headings | Space Grotesk | Titles |
| Font body | Inter | Body copy |

Dark theme only. No light mode.
