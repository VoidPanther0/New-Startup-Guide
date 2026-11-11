# Part 4 — Deploy

> **Deploy for $0.** Cloudflare Pages & Workers.
>
> Push to GitHub · Connect Domain · Workers for APIs · Global CDN · Free Email · Noindex.pages.dev

**Part 4 — Deploy**

---

## Deploy for $0 (Cloudflare Pages & Workers)

**Goal:** Website ko live karna — free, fast, aur apne `.com` domain pe.

| Card | Kya milega |
|------|-----------|
| **Push to GitHub** | Auto deploy on commit |
| **Connect Domain** | Free SSL, 1 click |
| **Workers for APIs** | 100k requests/day free |
| **Global CDN** | Fast everywhere, no server |
| **Free Email** | admin@yoursite.com → gmail |
| **Noindex.pages.dev** | Kill duplicate content |

---

## 1. GitHub CLI install (`gh`)

Official site: **https://cli.github.com/**

**Windows (WinGet):**

```powershell
winget install --id GitHub.cli --source winget
```

**macOS (Homebrew):**

```bash
brew install gh
```

**Linux — Debian / Raspberry Pi / Ubuntu:**

```bash
(type -p wget >/dev/null || (sudo apt update && sudo apt install wget -y)) \
	&& sudo mkdir -p -m 755 /etc/apt/keyrings \
	&& out=$(mktemp) && wget -nv -O$out https://cli.github.com/packages/githubcli-archive-keyring.gpg \
	&& cat $out | sudo tee /etc/apt/keyrings/githubcli-archive-keyring.gpg > /dev/null \
	&& sudo chmod go+r /etc/apt/keyrings/githubcli-archive-keyring.gpg \
	&& sudo mkdir -p -m 755 /etc/apt/sources.list.d \
	&& echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/githubcli-archive-keyring.gpg] https://cli.github.com/packages stable main" | sudo tee /etc/apt/sources.list.d/github-cli.list > /dev/null \
	&& sudo apt update \
	&& sudo apt install gh -y
```

**Linux — Fedora / RHEL (DNF5):**

```bash
sudo dnf install dnf5-plugins
sudo dnf config-manager addrepo --from-repofile=https://cli.github.com/packages/rpm/gh-cli.repo
sudo dnf install gh
```

**Linux — openSUSE / SUSE:**

```bash
sudo zypper addrepo https://cli.github.com/packages/rpm/gh-cli.repo
sudo zypper ref
sudo zypper install gh
```

**Verify:**

```bash
gh --version
```

> ⚠️ **Windows note:** installer PATH modify karta hai — **naya terminal window** kholna
> padega. Naya tab nahi, naya window.

---

## 2. Git + GitHub setup (ek hi baar)

```bash
git config --global user.name "Your Name"
git config --global user.email "you@example.com"
gh auth login
```

`gh auth login` → **GitHub.com** → **HTTPS** → **Login with a web browser** → code paste karo.

---

## 3. Git repo banao + push to GitHub

```bash
cd sachindev.com
git init
git add .
git commit -m "Initial commit"
gh repo create sachindev.com --public --source=. --remote=origin --push
```

Ab repo GitHub pe live hai. Cloudflare Pages ko isi repo se connect karoge →
**har commit pe auto deploy**.

---

## 4. Cloudflare pe Wrangler login

```bash
npx wrangler login
```

Browser khulega → apne Cloudflare account ko authorize karo.

> Global install chahiye toh: `npm install -g wrangler`

---

## 5. Cloudflare Deployment Prompts (AI se)

### 5.1 Pehle decide karo — Pages ya Workers?

```text
this website is static or not? and should i deploy it in cloudflare pages or workers ?
```

AI check karega ki `astro.config.mjs` mein `output: 'static'` hai ya `server`/`hybrid`,
aur bata dega Pages sahi hai ya Workers.

### 5.2 Cloudflare Pages ke liye

```text
I have already loggedin to cloudflare wrangler, now deploy this astrojs website to cloudflare pages and take info from astro docs mcp if needed. Also add deploy script to package.json
```

### 5.3 Cloudflare Workers ke liye

```text
I have already loggedin to cloudflare wrangler, now deploy this astrojs website to cloudflare workers and take info from astro docs mcp if needed. Also add deploy script to package.json
```

AI `package.json` mein ye add karega:

```json
{
  "scripts": {
    "dev": "astro dev",
    "build": "astro build",
    "preview": "astro preview",
    "deploy": "npm run build && wrangler pages deploy dist"
  }
}
```

> Workers variant mein deploy script `wrangler deploy` use karega, aur `wrangler.jsonc` /
> `wrangler.toml` bhi configure hoga.

---

## 6. Temporary domain ka indexing band karo (noindex)

Jab tak site `*.pages.dev` / `*.workers.dev` pe hai, asli domain connect nahi hua — do URLs
se same content live hoga (SEO split). Ye band karo.

### Agar CF Pages pe deploy kiya

`public/` folder mein `_headers` file banao aur apna **pages.dev URL** daal do:

```txt
https://project.pages.dev/*
  X-Robots-Tag: noindex
```

Preview branch URLs bhi cover karne ke liye (optional, official Cloudflare pattern):

```txt
https://:project.pages.dev/*
  X-Robots-Tag: noindex

https://:version.:project.pages.dev/*
  X-Robots-Tag: noindex
```

> Ye rule **sirf pages.dev URL pe** lagta hai — tumhare `.com` domain pe koi effect nahi.
> Isliye domain connect karne ke baad bhi rehne do. ✅
>
> Astro `<meta>` wala option bhi hai → [Noindex.pages.dev](#-noindexpagesdev-kill-duplicate-content)

### Agar CF Workers pe deploy kiya

Custom domain add karne ke baad **Workers settings mein workers domain OFF** kar do:

1. Dashboard → **Workers & Pages** → apna Worker
2. **Settings** → **Domains & Routes**
3. `workers.dev` → **Disable** → confirm

+ config mein bhi likho ( warna next deploy pe wapas enable ho jayega):

```toml
# wrangler.toml
workers_dev = false
```

---

## 7. Deploy 🚀

```bash
npm run deploy
```

Bas. Site live ho jayegi `*.pages.dev` pe — **bilkul free**.

**Manual tarika (agar `npm run deploy` na chal raha):**

```bash
npx wrangler pages project create
npx wrangler pages deploy dist
```

**Preview deploy (branch pe):**

```bash
npx wrangler pages deploy dist --branch=preview
```

**Drag & drop (dashboard se):** Workers & Pages → Create application → **Drag and drop
your files** → `dist/` folder drop karo → Deploy site.

> ⚠️ **Important:** Direct Upload (wrangler) ka use kar liya toh baad mein **Git
> integration pe switch nahi kar sakte** — naya project banana padega. Agar
> **auto-deploy on commit** chahiye toh Pages project dashboard se **"Connect to Git"**
> se banao, Direct Upload se nahi.

---

## 8. Domain connect karo (Cloudflare) 🌍

Deploy ke baad apna `.com` domain bhi connect karo. **Ye step har website ke liye same hai**
(Pages ho ya Workers).

### 8.1 Domain kharido (agar nahi kharida)

| Registrar | Link | Kyun |
|------------|------|------|
| **Hostinger** ⭐ | https://www.hostinger.com/in | **Recommended** — best price, hosting ke saath free domain |
| **Namecheap** | https://namecheap.pxf.io/c/7622160/386170/5618 | Cheap renewal + free WHOIS privacy |
| **Spaceship** | https://spaceship.sjv.io/c/7622160/1794549/21274 | Free WHOIS privacy + free email |
| **Spaceship (Promos)** 💰 | https://www.spaceship.com/promos/ | Running deals — sabse sasta `.com` |
| **BigRock** | https://www.bigrock.in/ | Indian registrar, `.com` sasta |
| **GoDaddy** | https://www.godaddy.com/en-in | UI easy, par renewal mehnga |

### 8.2 Cloudflare mein site add karo

1. **Websites** → **Add a site** → apna domain daalo → **Free** plan
   → https://dash.cloudflare.com/?to=/:account/add-site
2. Cloudflare 2 nameservers dega → **Activate** karo
   - ⚠️ Purane DNS records delete mat karna

### 8.3 Registrar mein nameservers change karo

Cloudflare ke 2 nameservers registrar mein paste karo (Namecheap → Manage → Nameservers →
Custom DNS · Spaceship → Nameservers · Hostinger → Nameservers · GoDaddy → Nameservers).

```bash
nslookup yourdomain.com   # Cloudflare ke nameservers dikhne chahiye
```

### 8.4 Pages / Workers se domain connect karo

**Pages:** Workers & Pages → project → **Custom domains** → **Set up a domain** →
`yourdomain.com` → DNS + SSL automatic

**Workers:** Settings → **Domains & Routes** → **Add Custom Domain** → `yourdomain.com`

### 8.5 `www` connect karo (updated way)

Apex domain ke saath `www` bhi connect hota hai — par dono alag URLs hote hain, toh ek ko
**301 redirect** karna zaroori hai:

| Type | Name | Content | Proxy |
|------|------|---------|-------|
| CNAME | `www` | `<project>.pages.dev` | **Proxied (orange cloud) ✅** |

Phir **Rules → Overview → Templates → "Redirect www to non-www"** use karo:
`http.host eq "www.yourdomain.com"` → `https://yourdomain.com` (Preserve path) · Status **301**.

> ⚠️ Redirect record **proxied** hona chahiye. Grey cloud pe rule kaam nahi karega.
> 📖 Details → [Part 5 — Cloudflare](Step5-Cloudfare.md) (Section 6)

### 8.6 Personal email (Cloudflare Email Service)

Apne domain pe apna email — `admin@yourdomain.com`:

| | Kya | Plan |
|---|---|---|
| **Email Routing** | mail aaye → gmail pe forward | ✅ **Free, unlimited** |
| **Email Sending** | apne domain se mail bhejo (contact form) | ⚠️ Workers Paid (Beta) |

**Routing (free):** Dashboard → **Compute** → **Email Service** → **Email Routing** →
**Onboard Domain** → **Destination Addresses** (gmail verify) → **Routing Rules** →
pattern `admin` → action *Send to an email* → gmail

**Sending:** Dashboard → **Compute** → **Email Service** → **Email Sending** →
**Onboard Domain** → phir Workers binding / REST API / SMTP se bhejo

> 📖 Details, code samples aur limits → [Part 5 — Cloudflare](Step5-Cloudfare.md) (Section 9)

### 8.7 SSL + duplicate content check

- SSL/TLS mode **Full (Strict)** → certificate **Active** → `https://yourdomain.com` ✅
- `workers.dev` / `pages.dev` indexing band (Section 6)

> 📖 **Details, troubleshooting table aur saare links:**
> **[Part 5 — Cloudflare](Step5-Cloudfare.md)**

---

## Pages vs Workers — kaunsa choose karein?

| Use **Pages** | Use **Workers** |
|----------------|-----------------|
| Static website (no backend logic) | Server-side logic chahiye |
| Database, APIs, authentication nahi | Database / APIs / auth chahiye |
| **Har simple micro-tool ke liye sahi choice** | Dynamic tools ke liye |

> **Zyadatar micro-tools ke liye Cloudflare Pages hi sahi hai.**

| Limit | Pages (Free) | Workers (Free) |
|-------|---------------|----------------|
| Requests | Unlimited bandwidth | **100,000/day** (uske baad pay) |
| Builds | 500/month | — |
| Files | 20,000 per site | 20,000 per Worker version |
| File size | 25 MiB | 25 MiB |
| CPU / Memory | — | 10 ms CPU, 128 MB |

---

## 🌐 Connect Domain (free SSL, 1 click)

**Pages:**

1. Cloudflare dashboard → **Workers & Pages** → apna project
2. **Custom domains** → **Set up a custom domain**
3. `yourdomain.com` daalo → DNS + SSL automatic (free edge certificate)
4. Verify → site live with **HTTPS** 🔒

**Workers:** Settings → **Domains & Routes** → **Add Custom Domain** → aur wahi
`workers.dev` **Disable** kar do (Part 3 ka checklist yaad karo).

---

## 🌍 Global CDN (free, no server)

Cloudflare ka CDN 300+ edge locations pe cache karta hai:

- Duniya bhar se fast load
- DDoS protection free
- HTTP/3 + Brotli compression
- Koi server manage nahi karna — **$0**

---

## 📧 Free Email (admin@yoursite.com → gmail)

Cloudflare **Email Routing** se apne domain pe professional email — free:

1. Dashboard → **Email** → **Email Routing** → Get started
2. **Destination address** add karo (tumhara gmail) → verify karo
3. **Custom addresses** mein `admin@yourdomain.com` banao → gmail pe forward
4. MX records Cloudflare khud set kar dega

> ⚠️ Ye **forwarding** hai — sending nahi. Aise address se mail bhejna hai toh
> Google Workspace / Zoho / Brevo jaisa paid sender chahiye. Receiving ke liye free hai.

---

## 🔍 Noindex.pages.dev (duplicate content khatam)

Jab tak site `*.pages.dev` pe hai, asli domain connect nahi hua — do URLs se same content
live hoga (SEO split). Isko rok do.

> ☝️ **Cloudflare `_headers` wala method** (bina rebuild ke, recommended) →
> [Section 6 — Temporary domain ka indexing band karo](#6-temporary-domain-ka-indexing-band-karo-noindex)

**Astro mein (recommended — khud-ba-khud):**

```astro
---
// src/layouts/BaseLayout.astro
const isPagesDev = Astro.url.hostname.endsWith('.pages.dev');
---
{isPagesDev && <meta name="robots" content="noindex, nofollow" />}
```

**Ya AI se:**

```text
Add a noindex meta tag to my site while it is hosted on the .pages.dev domain, so the
temporary URL doesn't compete with my real domain. I'll remove it later.
```

> `.com` connect hone ke baad ye automatically band ho jayega (hostname check).

---

## 🛒 Domain Buy Karna + Connect

Site `*.pages.dev` pe live ho chuki hai, ab asli `.com` domain kharido aur connect karo.

> 📖 **Poora walkthrough (nameservers → SSL → email → troubleshooting) ke liye dekho:**
> **[Part 5 — Cloudflare](Step5-Cloudfare.md)**

### 1. Registrar se domain kharido

| Registrar | Link | Kyun |
|------------|------|------|
| **Hostinger** ⭐ | https://www.hostinger.com/in | **Recommended** — best price, hosting ke saath free domain |
| **Namecheap** | https://namecheap.pxf.io/c/7622160/386170/5618 | Cheap renewal + free WHOIS privacy |
| **Spaceship** | https://spaceship.sjv.io/c/7622160/1794549/21274 | Free WHOIS privacy + free email |
| **Spaceship (Promos)** 💰 | https://www.spaceship.com/promos/ | Running deals — sabse sasta `.com` |
| **BigRock** | https://www.bigrock.in/ | Indian registrar, `.com` sasta |
| **GoDaddy** | https://www.godaddy.com/en-in | UI easy, par renewal mehnga |

> 💛 **Namecheap / Spaceship ke links affiliate hain** — unse domain kharidoge toh humara
> channel support milta hai aur aise **FREE videos banane ke liye** continue karne mein
> madad milti hai. ❤️

> ⚠️ **Renewal price check karo** (first-year nahi) + auto-renew ON + 2FA ON.

### 💰 Sabse sasta domain kaise milega

> 💰 **Promo page dekho pehle:** **https://www.spaceship.com/promos/**
> Spaceship ke promo page pe running deals milte hain — yahan `.com` first-year aur
> renewal dono mein sabse sasta price milta hai.

**AI se sasta domain dhundhwao (prompt):**

```text
Find the cheapest available .com domain for "[your keyword]". Check Spaceship promos first
(https://www.spaceship.com/promos/), then compare with Namecheap and Hostinger. Show the
first-year price AND the renewal price for each one, and tell me which is cheapest overall.
```

### 2. Cloudflare mein domain add karo

1. Cloudflare dashboard → **Websites** → **Add a site** → apna domain daalo
2. Plan **Free** select karo
3. Cloudflare 2 nameservers dega (e.g. `alice.ns.cloudflare.com`, `bob.ns.cloudflare.com`)
4. Registrar ke panel mein jaake **nameservers change** karo → Cloudflare wale daalo
5. Save → propagation (5 min – 48 hrs, usually 5–30 min)

### 3. Domain ko Pages/Workers se connect karo

**Pages:** Workers & Pages → project → **Custom domains** → **Set up a custom domain** →
`yourdomain.com` → SSL automatic (free edge certificate).

**Workers:** Settings → **Domains & Routes** → **Add Custom Domain** → `yourdomain.com`.

### 4. `workers.dev` / `pages.dev` band karo (duplicate content)

- **Pages** → `_headers` file mein `X-Robots-Tag: noindex` rule (Section 6)
- **Workers** → Settings → Domains & Routes → `workers.dev` → **Disable** + `workers_dev = false`

### 5. Free email (optional)

`admin@yourdomain.com` → gmail forwarding → [Free Email](#-free-email-adminyoursitecom--gmail)

---

## ✅ Deploy Checklist

- [ ] `gh --version` chal raha hai
- [ ] `git config --global user.name / user.email` set hain
- [ ] `gh auth login` done
- [ ] `npx wrangler login` done (browser authorize)
- [ ] Git repo bana + `gh repo create` se push
- [ ] `package.json` mein `deploy` script hai
- [ ] `npm run deploy` — site `*.pages.dev` pe live
- [ ] Temporary URL pe **noindex** laga hai
      (Pages → `_headers` mein `X-Robots-Tag: noindex` · Workers → `workers.dev` disabled)
- [ ] `.com` domain connected + HTTPS (free SSL) on
- [ ] Cloudflare nameservers registrar mein set hain
- [ ] Workers use kar rahe ho → `workers.dev` disabled + `workers_dev = false`
- [ ] Free email route set (optional) → [Personal email](#86-personal-email-cloudflare-email-service)
- [ ] **Domain kharida** — kyunki site live hai aur kaam kar rahi hai ✅

---

## Deliverable

A live website on your own `.com` domain — free SSL, global CDN, free email routing,
$0 hosting cost, auto-deploy on every commit.
