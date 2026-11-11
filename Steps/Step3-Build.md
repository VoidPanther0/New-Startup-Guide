# Part 3 — Build

> **AI Writes the Code. You describe it. It builds. Zero coding.**
>
> Free tools · Give it rules · Small steps · Test in browser · Internationalization

**Step in this part:** Step 4

---

## Step 4 — Build with AI (Write the Code)

**Goal:** Use AI to write the website code. No manual typing — you describe, it builds.

---

## The Stack

| Layer | Tool | Why |
|-------|------|-----|
| **builder** | [AI](https://opencode.ai/) · [Claude Code](https://claude.com/product/claude-code) | Zero Coding |
| **engine** | [Astro](https://astro.build/) | Max Speed & SEO |
| **hosting** | [Cloudflare](https://www.cloudflare.com/) | $0 Cost |

### 💡 Cloudflare tip — static site = 100% free

Agar tumhara site **static** hai (Astro ka default `output: 'static'`), toh Cloudflare pe
**paisa zero** lagega.

- **Cloudflare Workers (Static Assets)** — Free plan par **100,000 requests/day** milte hain
  (midnight UTC pe reset). Uske baad Cloudflare **Error 1027** deta hai aur paid plan chahiye.
  Source: [Workers limits](https://developers.cloudflare.com/workers/platform/limits/)
- **Cloudflare Pages** — Free plan par **500 builds/month**, **20,000 files/site**,
  **25 MiB** per file, **100 custom domains** aur **unlimited preview deployments**.
  Bandwidth **unlimited** hai — matlab 1000% free. Source: [Pages limits](https://developers.cloudflare.com/pages/platform/limits/)
- Pages Functions / Workers ka request quota **Workers wala hi** hai (100,000/day).

> **Rule:** Static banao (Astro default), bina login ke, bina database ke → Cloudflare free
> tier kabhi exceed nahi hoga.

---

## ✅ Pre-Build Checklist (device pe install hona chahiye)

| # | Tool | Install (Windows) | Install (macOS / Linux) | Why |
|---|------|-------------------|------------------------|-----|
| 1 | **Git** | `winget install --id Git.Git -e` | `brew install git` · `sudo apt install git` | Version control + GitHub push |
| 2 | **Node.js** (v20.19+ / v22.23.4+) | `winget install --id OpenJS.NodeJS.LTS -e` | `brew install node` · [nvm](https://github.com/nvm-sh/nvm) | Astro chahiye |
| 3 | **VS Code** | `winget install --id Microsoft.VisualStudioCode -e` | `brew install --cask visual-studio-code` | Editor |
| 4 | **GitHub CLI** (`gh`) | `winget install --id GitHub.cli --source winget` | `brew install gh` | Repo create + push (Part 4) |
| 5 | **Wrangler** (Cloudflare) | `npm install -g wrangler` | same | Deploy CLI (Part 4) |
| 6 | **Astro Extension** (VS Code) | Extensions panel → "Astro" → Install | same | Syntax + IntelliSense |
| 7 | **GitHub Account** | https://github.com/signup | same | Repo + auto deploy |
| 8 | **Cloudflare Account** | https://dash.cloudflare.com/sign-up | same | Free hosting |
| 9 | **AI CLI** | `npm i -g @anthropic-ai/claude-code` | same | Coding agent (OpenCode bhi chalega) |
| 10 | **Terminal** | Git Bash / PowerShell | Terminal | Commands |

**One-time setup commands:**

```bash
git config --global user.name "Your Name"
git config --global user.email "you@example.com"
gh auth login
npx wrangler login
```

> ⚠️ **Windows note:** `winget` install PATH modify karta hai — naya terminal window kholo.

> ⚠️ **VS Code mein pehle hi** Astro extension install kar lo — VS Code open karte hi
> extensions panel (Ctrl+Shift+X) mein "Astro" search karke install karo.

---

## Step-by-step build commands (copy → paste)

### 1. Domain ka folder banao (pehle!)

Folder ka naam **domain** ka naam hona chahiye, taaki baad mein Cloudflare Pages
mein "Production branch / Root directory" mein seedha ye folder point kar sako.

```bash
mkdir sachindev.com
cd sachindev.com
```

### 2. Astro template add karo (folder ke andar)

Ab tum `sachindev.com/` folder ke **andar** ho. Yahan Astro template banega:

```bash
npm create astro@latest . -- --template minimal --add tailwind --no-git --yes
```

> 🔹 `.` ka matlab hai **current folder** mein banao (isliye pehle `mkdir` + `cd` zaroori hai).
> 🔹 GitHub par repo root mein banana hai toh wahi `.` command chalega — bas repo root pe
> ho, ya `mkdir` step skip kar do.
> 🔹 `--add tailwind` se Tailwind v4 (Astro ka official integration) lag jata hai.
> 🔹 `--no-git` — kyunki git ek hi baar, repo root pe init hoga.

### 3. Astro Docs MCP server (AI ko latest docs dena)

Ye AI ko **real-time Astro documentation** deta hai, taaki outdated API use na kare.

**Claude Code:**

```bash
claude mcp add --transport http astro-docs https://mcp.docs.astro.build/mcp
```

**OpenCode** — `opencode.json` file mein:

```json
{
  "$schema": "https://opencode.ai/config.json",
  "mcp": {
    "astro-docs": {
      "type": "remote",
      "url": "https://mcp.docs.astro.build/mcp",
      "enabled": true
    }
  }
}
```

**Manual JSON (koi bhi tool):**

```json
{
  "mcpServers": {
    "astro-docs": {
      "type": "http",
      "url": "https://mcp.docs.astro.build/mcp"
    }
  }
}
```

> Docs: https://docs.astro.build/en/guides/build-with-ai/

### 4. AI skills install karo

Ye do skills dono agents ke liye kaam karti hain (Claude Code + OpenCode + 75 aur):

```bash
npx skills add https://github.com/lombiq/tailwind-agent-skills --skill tailwind-4-docs
npx skills add https://github.com/vercel-labs/agent-skills --skill web-design-guidelines
```

> `-a claude-code` / `-a opencode` add kar sakte ho jahan takar/consent puche.
> Check karo: `npx skills list`

### 5. DESIGN.md add karo (AI ko design rules do)

AI ka sabse bada problem "generic AI look" hota hai. `DESIGN.md` usse bol deta hai
**exactly kaunsa look chahiye** — colors, fonts, spacing, components, rules.

```bash
npx getdesign@latest add vercel
```

Ye `DESIGN.md` file banata hai project root mein (pehli baar → `./DESIGN.md`,
phir dusri design → `./<brand>/DESIGN.md`).

**Do jagah se designs milte hain:**

| Site | Link | Note |
|------|------|------|
| **OpenDesignMD** ⭐ | https://opendesignmd.com/ | **Naya + recommended** — 1,186 real sites, previews/screenshot ke saath dikhte hain. Download: `https://opendesignmd.com/designs/<slug>/DESIGN.md` |
| **getdesign.md** | https://getdesign.md/ | Purana (tumhara wala) — CLI `npx getdesign@latest add <brand>` |

> Brands list dekhne ke liye: `npx getdesign@latest list`
> (apple, stripe, vercel, linear, notion, supabase, figma, … sab hain)

**Agar koi aur design chahiye (OpenDesignMD se):**

```bash
curl -o DESIGN.md https://opendesignmd.com/designs/stripe/DESIGN.md
```

### 6. Dev server chalao

```bash
npm run dev
```

Browser mein `http://localhost:4321` khul jayega. Har change turant dikhega.

---

## ✍️ The Universal Prompt (Step 1 of building)

Claude Code (ya OpenCode) mein ye prompt paste karo — `[ ]` wali jagah apni values bharo:

```text
I have initialized a new AstroJS project. Use the astro-docs MCP, tailwind-4-docs skill,
and web-design-guidelines skill. Also use @DESIGN.md. Keep the website design like Vercel.

Name: [Your Tool Name]
Domain: [yourdomain.com]

Create a [describe your tool]. My competitor is [URL] — analyze it, identify its weaknesses,
and build a better version. Use MPA (multi-page application) architecture for best SEO.

First use plain mode (no styling, no animations) to make the structure and logic work.
Only after that, apply the DESIGN.md styling. Build it step by step and test in browser
after each step.
```

> **Press Enter.** AI competitor analyze karega, ideas banayega, aur website build karna
> shuru kar dega. Errors aate hi fix karega aur browser mein test bhi karega.

---

## 💡 Tips jo kaam aayenge

1. **Koi login mat rakho** — tool ko use karne ke liye sign-up/login force mat karo.
   Users aayenge, use karenge, chale jayenge. Simple = clean + fast = Google loves it.
2. **`npm run dev`** — AI ke build ke baad turant browser mein check karo.
3. **`/clear`** (Claude Code) — fresh session start karne ke liye. Token bachta hai.
4. **Problem precisely describe karo** — "generator bananao" mat bolo,
   "PDF se 12-column table nikaal ke Excel mein paste karne wala tool" bolo.
   AI usi hisaab se banata hai.
5. **AI ko bug do, fix khud karwao** — error paste karo, "fix this" bolo. Khud debug
   karne se zyada token nahi lagte.

### Common additions (is stage pe add karo)

- **Dark mode toggle** — modern websites ke liye essential.
- **Mobile responsiveness** — Chrome DevTools mein test karo
  (right-click → Inspect → mobile icon).
- **Multiple language support** — har language ke liye alag URL path, international SEO boost.
  Full guide + prompts → [🌐 Multi-language Support (i18n)](#-multi-language-support-i18n--international-seo)

---

## 🔍 On-Page SEO Optimization

```text
Do the on-page SEO of this website for:
Main Keyword: [your keyword]
Supporting Keywords: [comma-separated list from Ahrefs]

Also add proper OG meta tags. Write 600 words about the tool on the home page for SEO.
```

> ⚠️ **Home page ka 600-word section bahut zaroori hai.** Google ko samajhna hota hai
> page kis baare mein hai — sirf tool pages (bina text ke) almost kabhi rank nahi karte.

**Also ensure:**

- `<title>` tag mein primary keyword ho
- `<meta description>` 160 characters se kam ho aur keyword contain kare
- Saari images ke descriptive `alt` attributes hon
- Page URL clean ho (e.g. `/online-ruler`, nahi `/page?id=123`)

---

## ❓ FAQ Section (SEO ke liye)

```text
Add an SEO-friendly FAQ section using JSON-LD structured data for these questions:
[paste your questions from Ahrefs "Questions" tab and Google's "People Also Ask"]
```

> **Note:** JSON-LD use karo FAQ SEO ke liye — ye Google ko batata hai ki ye answers
> page par hain.

**Example JSON-LD:**

```html
<script type="application/ld+json">
{
 "@context": "https://schema.org",
 "@type": "FAQPage",
 "mainEntity": [{
 "@type": "Question",
 "name": "How to find an apprenticeship?",
 "acceptedAnswer": {
 "@type": "Answer",
 "text": "<p>We provide an official service to search through available apprenticeships. To get started, create an account here, specify the desired region, and your preferences. You will be able to search through all officially registered open apprenticeships.</p>"
 }
 }, {
 "@type": "Question",
 "name": "Whom to contact?",
 "acceptedAnswer": {
 "@type": "Answer",
 "text": "You can contact the apprenticeship office through our official phone hotline above, or with the web-form below. We generally respond to written requests within 7-10 days."
 }
 }]
}
</script>
```

---

## 📱 Mobile Responsive

```text
Make the webside mobile responsive
```

---

## 🌐 Multi-language Support (i18n) — International SEO

**Official recipe:** https://docs.astro.build/en/recipes/i18n/
**Built-in i18n routing:** https://docs.astro.build/en/guides/internationalization/

Ek hi tool 8 languages me rank kar sakta hai — har language ka apna URL path + hreflang.

### AI prompt (seedha paste karo)

```text
Use the Astro i18n recipe (https://docs.astro.build/en/recipes/i18n/) to provide multi lang
support to this website so that it ranks on other languages keywords.

provide support for:
Español
日本語
Français
Deutsch
Português
한국어
Italiano

note: Add hreflang markup for each language.
```

### 1. `astro.config.mjs` — i18n config

```js
import { defineConfig } from 'astro/config';

export default defineConfig({
  site: 'https://yourdomain.com',
  i18n: {
    locales: ['en', 'es', 'ja', 'fr', 'de', 'pt', 'ko', 'it'],
    defaultLocale: 'en',
    routing: { prefixDefaultLocale: false },
  },
});
```

### 2. Folder structure (har language ka folder)

```
src/pages/
├── index.astro        ← English (default, URL pe /en nahi)
├── es/                ← Español
├── ja/                ← 日本語
├── fr/                ← Français
├── de/                ← Deutsch
├── pt/                ← Português
├── ko/                ← 한국어
└── it/                ← Italiano
```

### 3. hreflang markup (`BaseLayout.astro` head)

```astro
---
import { getRelativeLocaleUrl } from 'astro:i18n';

const locales = ['en', 'es', 'ja', 'fr', 'de', 'pt', 'ko', 'it'];
const lang = Astro.currentLocale ?? 'en';
---
<html lang={lang}>
  <head>
    {locales.map((l) => (
      <link
        rel="alternate"
        hreflang={l}
        href={new URL(getRelativeLocaleUrl(Astro.url.pathname, l), Astro.site)}
      />
    ))}
    <link rel="alternate" hreflang="x-default" href={new URL(Astro.url.pathname, Astro.site)} />
  </head>
</html>
```

### 4. Language codes

| Language | Code | hreflang |
|----------|------|----------|
| English | `en` | `en` |
| Español | `es` | `es` |
| 日本語 | `ja` | `ja` |
| Français | `fr` | `fr` |
| Deutsch | `de` | `de` |
| Português | `pt` | `pt-BR` (Brazil focus) |
| 한국어 | `ko` | `ko` |
| Italiano | `it` | `it` |

### 5. i18n rules (SEO ke liye zaroori)

- ✅ `<html lang="...">` har page pe sahi language
- ✅ hreflang **reciprocal** hona chahiye — har page apne saare versions ko point kare
- ✅ `x-default` default/fallback page ke liye
- ✅ Same slug sab languages me (path same, prefix alag) — URL structure consistent rehta hai
- ✅ Sitemap me **sab languages** include karo (`@astrojs/sitemap` i18n support karta hai)
- ✅ **Local keywords translate karo** — sirf UI nahi:
  `"online ruler"` → `règle en ligne` · `regla en línea` · `线上定規` · `온라인 자` · `regua online`
- ⚠️ **Machine translation alone mat karo** — Google low-value translated content detect kar
  leta hai. Native-level translation + local keyword research zaroori hai.

---

## 🎯 Single Language Website Convert (optional)

Agar tumhe **sirf ek language** chahiye (e.g., sirf Brazil + Portuguese countries me rank),
toh poori website ka content hi convert kar do — `/lang/` pages nahi.

### AI prompt (seedha paste karo)

```text
this is my english website, convert this full website to rank on "Keywoard Name" and domain : [Name Domain]] we want to rank in brazil and every portuguese countries.

Add hreflang markup of [Languages].

NOTE: Do not just give a /lang pages, convert this whole website to a [Name] website only. we only want [Name] language no other languages.

Change the full content of this website to rank on main keyword: [Keywoard]
Supportive keywords: [....]
```

### Kab kaunsa approach

| Situation | Approach |
|-----------|----------|
| Har market ka **alag domain** hai (`br.com`, `es.com`) | ✅ **Single-language convert** per domain |
| Ek hi domain pe 8 languages | ✅ **Multi-language i18n** (upar wala section) |
| Sirf **ek country** target karna hai | ✅ **Single-language convert** (same domain, no `/lang/`) |

### Convert ke baad checklist

- [ ] `<html lang="pt-BR">` set (sahi language code)
- [ ] hreflang sirf usi language ka + `x-default`
- [ ] `<title>` + `<meta description>` **local keyword** se (English keyword translate karke mat rakhna)
- [ ] 600-word home content translated + localized (R$, cm, dd/mm/yyyy)
- [ ] FAQ + JSON-LD translated
- [ ] Images ke `alt` text translated
- [ ] `sitemap.xml` + `robots.txt` updated
- [ ] `canonical` tag sahi URL pe
- [ ] `astro.config.mjs` me `site` + `i18n.defaultLocale` update

---

## 🎨 Favicon banao

1. Logo banao — https://logofa.st/
2. Favicon ka zip banao — https://realfavicongenerator.net/
3. Zip ke andar ki files project ke `public/` folder mein rakho:
   `favicon.96x96.png`, `favicon.svg`, `favicon.ico`, `apple-touch-icon.png`,
   `site.webmanifest`
4. Ye prompt AI ko do:

```text
Add this favicon and svg icon as logo in my website:

<link rel="icon" type="image/png" href="/favicon-96x96.png" sizes="96x96" />
<link rel="icon" type="image/svg+xml" href="/favicon.svg" />
<link rel="shortcut icon" href="/favicon.ico" />
<link rel="apple-touch-icon" sizes="180x180" href="/apple-touch-icon.png" />
<link rel="manifest" href="/site.webmanifest" />
```

---

## 📄 Required Pages (AdSense Approval ke liye)

```text
Create these pages as separate MPA routes for best SEO:
- Privacy Policy
- Terms & Conditions
- About Us
- Contact Us

Make these pages visible and linked in the home page footer and header.

This website should be a multi-page application (MPA) for best SEO.
```

> ⚠️ Ye pages **home page se clearly accessible** hone chahiye — AdSense reviewers ye
> check karte hain. Header **aur** footer dono mein link karo.
>
> 📖 AdSense approval ke liye poora checklist → **[Part 7 — Earnings](Step7-Earnings.md)**

---

## 🚨 Error Pages (404, 500)

```text
Add error pages (404, 500) so broken URLs don't show the default Astro error screen
```

---

## 🤖 robots.txt

```text
Add robots.txt with a sitemap link
```

---

## 🗺️ sitemap.xml

```text
Create sitemap.xml for this website. Use the astro-docs MCP server for checking the
correct way to add it.
```

---

## 📊 Google Analytics (GA4)

```text
Add Google Analytics 4 (GA4) to this Astro site using gtag.js. Use my measurement ID
G-XXXXXXXXXX. Put the script in the shared BaseLayout head so it loads on every page,
load it with async, and don't inject it twice.
```

```html
<!-- Google tag (gtag.js) -->
<script async src="https://www.googletagmanager.com/gtag/js?id=G-XXXXXXXXXX"></script>
<script>
  window.dataLayer = window.dataLayer || [];
  function gtag(){dataLayer.push(arguments);}
  gtag('js', new Date());
  gtag('config', 'G-XXXXXXXXXX');
</script>
```

> Measurement ID kahan se milega: Google Analytics → Admin → Property → Data Streams →
> **Web → Measurement ID** (`G-XXXXXXXXXX`).
>
> 📖 Poora GA4 setup + AdSense linking guide → **[Part 6 — Search Console](Step6-SearchConsole.md)**

---

## 🛡️ Cloudflare `_headers` file (security + cache rules)

```text
Create a `public/_headers` file for Cloudflare. Add security headers for all routes
(X-Frame-Options, X-Content-Type-Options, Referrer-Policy, Permissions-Policy,
Strict-Transport-Security) and long cache-control for static assets.
```

```bash
# public/_headers
/*
  X-Frame-Options: DENY
  X-Content-Type-Options: nosniff
  Referrer-Policy: strict-origin-when-cross-origin
  Permissions-Policy: geolocation=(), camera=(), microphone=()
  Strict-Transport-Security: max-age=31536000; includeSubDomains; preload

/_astro/*
  Cache-Control: public, max-age=31536000, immutable
```

> File `public/` folder mein honi chahiye — Astro build ke baad `dist/` mein copy ho jayegi.
>
> 💡 `pages.dev` URL ko Google se index hone se rokne ke liye isi file mein
> `X-Robots-Tag: noindex` rule add karo → [Part 4 — Deploy](Step4-Deploy.md)

### 🔒 Workers use kar rahe ho? `workers.dev` band karo

Jab `.com` domain connect ho jaye, toh `workers.dev` URL **disable** kar do — warna do jagah
se site live rahegi (duplicate content + SEO split).

**Dashboard se:**

1. Cloudflare dashboard → **Workers & Pages**
2. Apna Worker select karo → **Settings** → **Domains & Routes**
3. `workers.dev` ke paas **Disable** pe click karo → confirm

**Wrangler config se (zaroori!):**

```toml
# wrangler.toml
workers_dev = false
```

```jsonc
// wrangler.jsonc
{
  "workers_dev": false
}
```

> ⚠️ **Warning:** Agar dashboard se disable kiya par `workers_dev = false` config mein nahi
> likha, toh **next deploy par wapas enable** ho jayega. Dono karo.

---

## ✅ Final Checklist (ship se pehle)

- [ ] **Website banaya** — Universal Prompt se, competitor URL diya hua
- [ ] **Logo banaya** — https://logofa.st/
- [ ] **Favicon banaya** — https://realfavicongenerator.net/ → `public/` mein files + link tags
- [ ] **Mobile responsive** — Chrome DevTools (right-click → Inspect → mobile icon) pe test
- [ ] **SEO done** — 600-word content + `<title>` + `<meta description>` + OG tags
- [ ] **FAQ section** — questions + JSON-LD `FAQPage` schema
- [ ] **Pages banaye** — Privacy Policy, About Us, Terms & Conditions, Contact Us (header + footer dono mein linked)
- [ ] **Error pages** — custom 404 aur 500
- [ ] **robots.txt** — sitemap link ke saath
- [ ] **sitemap.xml** — Astro ke sahi tareeke se
- [ ] **Google Analytics** — GA4 (gtag.js) lagaya, har page pe chalta hai
- [ ] **`_headers` file** — `public/_headers` mein security + cache rules
- [ ] **Workers use kar rahe ho?** — `.com` connect ke baad `workers.dev` disable +
      `workers_dev = false` config mein likha
- [ ] `npm run build` — bina error pass ho raha hai
- [ ] `npm run dev` — mobile + desktop dono pe sahi dikh raha hai
- [ ] Dark mode toggle kaam kar raha hai
- [ ] Multiple languages (agar chahiye toh) alag URLs pe hain + hreflang markup + `<html lang>` sahi
- [ ] Single-language convert (agar target ek country hai) — poora content localized, `/lang/` nahi
- [ ] Git push ho gaya hai GitHub pe
- [ ] Cloudflare se live deploy ho gaya hai
- [ ] **Domain tabhi kharida** jab site live aur working ho

---

## Deploy (Cloudflare)

Ye poora flow (GitHub push → Pages → domain connect) **[Part 4 — Deploy](Step4-Deploy.md)**
mein hai. Quick peek:

```bash
npx wrangler@latest deploy
```

Ya Cloudflare dashboard se **Workers & Pages → Create → Connect to Git** — har push pe
auto build + deploy ho jayega.

---

## Deliverable

A working website, ready to go live — built by AI, at $0 cost.

**Next:** [Step 5 — Deploy for $0](Step4-Deploy.md) (Part 4)
