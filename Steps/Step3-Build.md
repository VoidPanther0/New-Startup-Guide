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

| # | Tool | Install / Verify | Why |
|---|------|------------------|-----|
| 1 | **Git** | `winget install --id Git.Git -e` | Version control + GitHub push |
| 2 | **Node.js** (v20.19+ / v22.23.4+) | `winget install --id OpenJS.NodeJS.LTS -e` | Astro chahiye |
| 3 | **VS Code** | `winget install --id Microsoft.VisualStudioCode -e` | Editor |
| 4 | **Astro Extension** (VS Code) | Extensions panel → "Astro" → Install | Syntax + IntelliSense |
| 5 | **GitHub Account** | https://github.com/signup | Repo + Pages/Workers deploy |
| 6 | **Cloudflare Account** | https://dash.cloudflare.com/sign-up | Free hosting |
| 7 | **AI CLI** | `npm i -g @anthropic-ai/claude-code` ya OpenCode | Coding agent |
| 8 | **Terminal** | Git Bash / PowerShell | Commands |

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

## ✅ Final Checklist (ship se pehle)

- [ ] `npm run build` — bina error pass ho raha hai
- [ ] `npm run dev` — mobile + desktop dono pe sahi dikh raha hai
- [ ] Dark mode toggle kaam kar raha hai
- [ ] 600-word SEO content home page pe hai
- [ ] `<title>` + `<meta description>` + OG tags set hain
- [ ] FAQ section + JSON-LD schema valid hai
- [ ] Privacy Policy, Terms, About Us, Contact Us pages hain aur linked hain
- [ ] 404 / 500 error pages custom hain
- [ ] `robots.txt` + `sitemap.xml` hain
- [ ] Favicon + logo lag gaya hai
- [ ] Multiple languages (agar chahiye toh) alag URLs pe hain
- [ ] Git push ho gaya hai GitHub pe
- [ ] Cloudflare se live deploy ho gaya hai
- [ ] **Domain tabhi kharida** jab site live aur working ho

---

## Deploy (Cloudflare)

```bash
npx wrangler@latest deploy
```

Ya Cloudflare dashboard se **Workers & Pages → Create → Connect to Git** — har push pe
auto build + deploy ho jayega.

---

## Deliverable

A working website, live on your `.com` domain — built by AI, hosted on Cloudflare, at $0 cost.
