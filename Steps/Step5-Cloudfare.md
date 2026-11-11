# Part 5 — Cloudflare

> **Universal Cloudflare guide.** Deploy · Connect Domain · Free SSL · Email · DNS · $0 cost.
>
> Ye part **har website ke liye same** hai — Pages ho ya Workers, logic aur links ek hi rehte hain.

**Step in this part:** Step 6

---

## Step 6 — Cloudflare (Deploy + Connect Domain)

**Goal:** Website ko Cloudflare pe deploy karna, apna `.com` domain connect karna, free SSL
aur free email lena — sab **$0** mein.

### Flow at a glance

1. **Cloudflare account** (free) + site add karo
2. **Deploy** — Pages ya Workers ([Part 4](Step4-Deploy.md))
3. **Domain kharido** (registrar se)
4. **Nameservers** Cloudflare ke set karo
5. **Domain connect** — Pages/Workers se
6. **`www` connect** — CNAME + 301 redirect (ek canonical URL)
7. **SSL verify** (auto on)
8. **`workers.dev` / `pages.dev` band** karo (duplicate content)
9. **Personal email** — `admin@yourdomain.com` (Routing free · Sending on Paid)
10. **Search Console** mein submit (optional)

---

## 1. Cloudflare account + site add karo

1. https://dash.cloudflare.com/sign-up → free account banao
2. **Websites** → **Add a site** → apna domain daalo
   → https://dash.cloudflare.com/?to=/:account/add-site
3. Plan **Free** select karo (₹0 — Cloudflare domain bhi free hai)
4. Cloudflare 2 nameservers dega (e.g. `alice.ns.cloudflare.com`, `bob.ns.cloudflare.com`)
5. DNS scan → **Review** → **Activate**
   - ⚠️ **Existing DNS records DELETE mat karna** (purane MX / email / subdomains) — warna
     email aur purani services band ho jayengi

> 💡 Domain abhi kharida nahi hai? Pehle registrar se kharido (Step 3) — phir wapas yahan.

---

## 2. Deploy (Pages ya Workers)

Poora deploy flow + AI prompts → **[Part 4 — Deploy](Step4-Deploy.md)** (Step 5)

| | Pages | Workers |
|---|---|---|
| Best for | Static micro-tools | Server-side logic, DB, API, auth |
| Requests | Unlimited bandwidth | 100,000/day free, uske baad pay |
| Deploy | `npm run deploy` | `npm run deploy` (`wrangler deploy`) |

**Deploy ke baad** → wapas yahan (Step 4 — domain connect).

---

## 3. Domain kharido (agar abhi nahi kharida)

| Registrar | Link | Kyun |
|------------|------|------|
| **Hostinger** ⭐ | https://www.hostinger.com/in | **Recommended** — best price, hosting ke saath free domain |
| **Hostinger (Domain Search)** | https://www.hostinger.com/in/domain-name-search | Direct domain search |
| **Namecheap** | https://namecheap.pxf.io/c/7622160/386170/5618 | Cheap renewal + free WHOIS privacy |
| **Spaceship** | https://spaceship.sjv.io/c/7622160/1794549/21274 | Free WHOIS privacy + free email |
| **Spaceship (Promos)** 💰 | https://www.spaceship.com/promos/ | Running deals — sabse sasta `.com` |
| **BigRock** | https://www.bigrock.in/ | Indian registrar, `.com` sasta |
| **GoDaddy** | https://www.godaddy.com/en-in | UI easy, par renewal mehnga |

> 💛 **Namecheap / Spaceship ke links affiliate hain** — unse domain kharidoge toh humara
> channel support milta hai aur aise **FREE videos banane ke liye** continue karne mein
> madad milti hai. ❤️

> 💰 **Sabse sasta?** → https://www.spaceship.com/promos/

```text
Find the cheapest available .com domain for "[your keyword]". Check Spaceship promos first
(https://www.spaceship.com/promos/), then compare with Namecheap and Hostinger. Show the
first-year price AND the renewal price for each one, and tell me which is cheapest overall.
```

> ⚠️ **Renewal price** check karo (first-year nahi) + auto-renew ON + 2FA ON.
> 💡 **Cloudflare Registrar** (at-cost, zero markup) → https://www.cloudflare.com/products/registrar/

---

## 4. Registrar mein nameservers change karo

Cloudflare ke 2 nameservers registrar ke panel mein paste karo:

| Registrar | Kahan milega |
|------------|--------------|
| Namecheap | Domain List → Manage → Nameservers → **Custom DNS** |
| Spaceship | Domain Portfolio → domain → **Nameservers** → Custom |
| Hostinger | Domains → Manage → **Nameservers** → Change |
| GoDaddy | My Domains → Manage → **Nameservers** → Change |
| BigRock | My Domains → Manage → **Nameservers** |

> Propagation: **5 min – 48 hrs** (usually 5–30 min).
> Docs: https://developers.cloudflare.com/dns/zone-setups/full-setup/setup/

Check karo:
```bash
nslookup yourdomain.com
```
Jab Cloudflare ke nameservers dikh jayein → propagation ho gaya.

---

## 5. Domain ko Pages / Workers se connect karo

**Cloudflare Pages:**

1. Workers & Pages → apna project → **Custom domains** → **Set up a domain**
2. `yourdomain.com` daalo → Continue
3. DNS + SSL **automatic** (CNAME khud ban jata hai)
   → https://developers.cloudflare.com/pages/configuration/custom-domains/

> **Apex domain** (`yourdomain.com`) ke liye nameservers Cloudflare ke honge hi padenge —
> warna `522` error aayega. **Subdomain** (`www.yourdomain.com`) ke liye manually CNAME
> record bana sakte ho → `<PROJECT>.pages.dev`.

**Cloudflare Workers:**

Settings → **Domains & Routes** → **Add Custom Domain** → `yourdomain.com`
→ https://developers.cloudflare.com/workers/configuration/routing/custom-domains/

**DNS record (agar manually banana ho):**

| Type | Name | Content | Proxy |
|------|------|---------|-------|
| CNAME | `@` | `<PROJECT>.pages.dev` | Proxied (orange cloud) ✅ |
| CNAME | `www` | `<PROJECT>.pages.dev` | Proxied (orange cloud) ✅ |

---

## 6. 🌐 `www` aur subdomains connect karna (updated way)

Apex domain (`yourdomain.com`) connect karte hi Cloudflare `www` ke liye DNS record
**automatically** bana deta hai. Lekin `www.yourdomain.com` aur `yourdomain.com` do alag URLs
hain → SEO me **duplicate content**. Isliye **ek ko canonical banao, doosre ko 301 redirect**.

### 6.1 `www` ka DNS record banao

**Pages:** Workers & Pages → project → **Custom domains** → **Set up a domain** →
`www.yourdomain.com` → Cloudflare CNAME khud bana dega.

**Workers:** Settings → **Domains & Routes** → **Add Custom Domain** → `www.yourdomain.com`.

**Manually (DNS tab):**

| Type | Name | Content | Proxy |
|------|------|---------|-------|
| CNAME | `www` | `<project>.pages.dev` | **Proxied (orange cloud) ✅** |

> ⚠️ **Redirect rule ke liye record PROXIED hona zaroori hai** (orange cloud). Grey cloud
> (DNS only) pe rule kaam nahi karega — Cloudflare request pehle dekhta hai, phir redirect
> karta hai.
> Docs: https://developers.cloudflare.com/dns/proxy-status/

### 6.2 Canonical choose karo + 301 redirect banao

Dashboard → **Rules** → **Overview** → **Templates** → **"Redirect www to non-www"** template
use karo (ya manually Single Redirect banao):

| Field | Value |
|-------|-------|
| Rule name | `www to apex` |
| **If** | `http.host eq "www.yourdomain.com"` |
| **Then** | Static → `https://yourdomain.com` + **Preserve path & query** ✅ |
| Status | **301** (permanent) |

**Wildcard version** (saare `www.` subdomains ek saath):

```
If:   http.host contains "www."
Then: Dynamic URL → concat("https://yourdomain.com", http.request.uri.path)
```

> 🔁 **Ulta chahiye (apex → www)?** Same rule, bas ulta:
> `http.host eq "yourdomain.com"` → `https://www.yourdomain.com` + preserve path.

> 💡 **Template vs manual:** Template sabse aasan hai. Manual tab chahiye jab same rule
> multiple zones mein chahiye ya custom logic ho.

### 6.3 Verify karo

- Browser me `https://www.yourdomain.com` kholo → address bar me **apex** dikhna chahiye
- Redirect kaam nahi kar raha? **Rules simulator** se test karo:
  → https://developers.cloudflare.com/rules/simulator/

### 6.4 Canonical tag (Astro mein)

```astro
---
// src/layouts/BaseLayout.astro
const canonical = new URL(Astro.url.pathname, Astro.site ?? Astro.url.origin).href;
---
<link rel="canonical" href={canonical} />
```

`astro.config.mjs` me bhi site URL set karo (sitemap + OG tags sahi aayenge):

```js
// astro.config.mjs
export default defineConfig({
  site: 'https://yourdomain.com',
});
```

### 6.5 `www` ya apex — kaunsa?

| | `www.yourdomain.com` | `yourdomain.com` (apex) |
|---|---|---|
| Common hona | zyada | kam |
| Look | thoda lamba | short & clean |
| SSL | same (free) | same (free) |
| Typing | easy | sabse easy |

> ✅ Dono theek hain — **pick one, redirect the other 301.** (Zyadatar log `www` choose karte
> hain, par `apex` bhi bilkul sahi hai.)

### 6.6 Redirect products & free limits

| Product | Free plan | Kab use karna hai |
|---------|-----------|-------------------|
| **Single Redirects** | 10 rules · wildcard ✅ · regex ❌ | `www` ↔ apex, ek-do rules |
| **Bulk Redirects** | 15 rules · 5 lists · 10,000 redirects | `*.pages.dev` → apna domain |

Dono **sabhi plans** pe available hain, aur dono ko DNS record **proxied** hona chahiye.
→ https://developers.cloudflare.com/rules/url-forwarding/

```text
# pages.dev ko apne domain pe redirect karo (Bulk Redirect)
Source:  https://<project>.pages.dev/*
Target:  https://yourdomain.com/$1
Status:  301
```

---

## 7. SSL verify karo

- Dashboard → **SSL/TLS** → **Overview** → Mode: **Full (Strict)**
- Certificate status: **Active**
- Browser mein check: `https://yourdomain.com` → 🔒 padlock

> SSL **automatic** hai Cloudflare se — kuch karna nahi padta, bas wait karna padta hai
> (usually 15 min – 24 hrs).
> Docs: https://developers.cloudflare.com/ssl/

---

## 8. Duplicate content band karo (noindex)

| Deploy kahan kiya | Kya karo |
|-------------------|----------|
| **Pages** | `public/_headers` mein `X-Robots-Tag: noindex` rule (sirf pages.dev URL pe) |
| **Workers** | Settings → Domains & Routes → `workers.dev` → **Disable** + `workers_dev = false` config |

```txt
# public/_headers  (Pages)
https://project.pages.dev/*
  X-Robots-Tag: noindex
```

Pages ke liye optional — `*.pages.dev` ko apne domain pe redirect karo:
**Rules → Bulk Redirect** → `https://<project>.pages.dev/*` → `https://yourdomain.com/$1`

> Workers static assets: https://developers.cloudflare.com/workers/static-assets/

---

## 9. 📧 Personal Email on Cloudflare (Email Service)

Apne domain pe apna email address — `admin@yourdomain.com`, `support@yourdomain.com`.
Cloudflare ke **Email Service** mein 2 products hain:

| | **Email Routing** (inbound) | **Email Sending** (outbound) |
|---|---|---|
| Kya karta hai | Aane wale mail ko **forward** / Worker ko de | Apne domain se mail **bhejta** hai |
| Plan | ✅ **Free + Paid** | ⚠️ **Workers Paid** (Beta) |
| Cost | **Unlimited, free** | 3,000/month included, phir **$0.35 per 1,000** |
| Setup | Email Routing | Email Sending |

> 💡 **Free trick:** Apne account ke **verified destination addresses** (tumhara gmail) ko mail
> bhejna **sabhi plans pe FREE** hai — quota bhi nahi lagta.

---

### 9.A Email Routing — `admin@yourdomain.com` → gmail (FREE)

**Receive** karna hai (mail aaye aur tumhe gmail pe mil jaye) → ye sahi hai.

1. Dashboard → **Compute** → **Email Service** → **Email Routing**
   → https://dash.cloudflare.com/?to=/:account/email-service/routing
2. **Onboard Domain** → Cloudflare MX + SPF + DKIM records add karega → **Done**
3. **Destination Addresses** → apna gmail daalo → Cloudflare ka verification mail aayega →
   **Verify email address**
4. **Routing Rules** → **Create routing rule**:
   - **Email pattern**: `admin` (ya `*` — sab kuch) + domain select
   - **Action**: *Send to an email*
   - **Destination**: tumhara gmail → **Save**
5. Test: kisi doosre account se `admin@yourdomain.com` pe mail bhejo → gmail me aana chahiye

**Bonus — auto-reply (Worker):**

```text
Add an email Worker to my Cloudflare project that auto-replies to incoming emails
with a short thank-you message and forwards a copy to my gmail.
```

> ⚠️ **Important:** Routing = **receive + forward** hi hai. Is address se **khud mail bhejne**
> ke liye neeche wala **Email Sending** chahiye.

Docs: https://developers.cloudflare.com/email-service/get-started/route-emails/

---

### 9.B Email Sending — apne domain se mail bhejna (Beta)

**Contact form / notifications** apne domain se bhejne hain → ye chahiye.

1. Dashboard → **Compute** → **Email Service** → **Email Sending**
   → https://dash.cloudflare.com/?to=/:account/email-service/sending
2. **Onboard Domain** → Cloudflare `cf-bounce` subdomain pe MX + SPF + DKIM + **DMARC**
   records add karega → **Done**
3. Ab 3 tarike se mail bhej sakte ho:

**① Workers binding** (Cloudflare Workers apps ke liye):

```jsonc
// wrangler.jsonc
{
  "send_email": [
    { "name": "EMAIL", "remote": true }
  ]
}
```

```ts
// src/index.ts
const YOUR_DOMAIN = "yourdomain.com";

export default {
  async fetch(request: Request, env: Env): Promise<Response> {
    const response = await env.EMAIL.send({
      to: "recipient@example.com",
      from: `welcome@${YOUR_DOMAIN}`,
      subject: "Welcome to our service!",
      html: "<h1>Welcome!</h1><p>Thanks for signing up.</p>",
      text: "Welcome! Thanks for signing up.",
    });
    return new Response(`Email sent: ${response.messageId}`);
  },
} satisfies ExportedHandler<Env>;
```

**② REST API** (kisi bhi platform se — Astro Pages bhi):

```bash
curl "https://api.cloudflare.com/client/v4/accounts/{account_id}/email/sending/send" \
  --header "Authorization: Bearer <API_TOKEN>" \
  --header "Content-Type: application/json" \
  --data '{
    "to": "recipient@example.com",
    "from": "welcome@yourdomain.com",
    "subject": "Welcome to our service!",
    "html": "<h1>Welcome!</h1><p>Thanks for signing up.</p>",
    "text": "Welcome! Thanks for signing up."
  }'
```

**③ SMTP** (Outlook / Gmail app / koi bhi mail client):

```sh
cat > mail.txt <<EOF
From: welcome@yourdomain.com
To: recipient@example.com
Subject: Welcome to our service!

Thanks for signing up.
EOF

curl --ssl-reqd \
  --url "smtps://smtp.mx.cloudflare.net:465" \
  --user "api_token:<API_TOKEN>" \
  --mail-from "welcome@yourdomain.com" \
  --mail-rcpt "recipient@example.com" \
  --upload-file mail.txt
```

**AI se contact form banana (prompt):**

```text
Add a contact form to this Astro site that sends email using Cloudflare Email Service
via the REST API. Use my API token and my domain (yourdomain.com). Store the token as an
environment variable, validate the input, and show a success message.
```

Docs: https://developers.cloudflare.com/email-service/get-started/send-emails/

---

### 9.C Limits (taaki baad me surprise na ho)

| Cheez | Limit |
|-------|-------|
| Recipients per email | 50 (to + cc + bcc combined) |
| Subject line | 998 characters |
| Total message size | 25 MiB (attachments samet) |
| Header size | 16 KB |
| Routing rules per domain | 200 |
| Destination addresses per account | 200 |
| Domains per zone | 30 |
| Outbound emails (Paid) | 3,000/month included, phir $0.35/1,000 |
| Inbound emails | **Unlimited** |

> New accounts ka daily sending quota initially conservative hota hai aur behaviour
> improve hone pe automatically badhta hai.

Docs: https://developers.cloudflare.com/email-service/platform/limits/ ·
Pricing: https://developers.cloudflare.com/email-service/platform/pricing/

---

### 9.D Deliverability tip

- SPF + DKIM + DMARC — teeno onboarding ke time **automatic** set hote hain (bina domain
  onboard kiye sending allowed nahi hai)
- Spam folder check karna mat bhoolna — first mail aksar spam me jaata hai
- Link: https://developers.cloudflare.com/email-service/concepts/deliverability/

---

## 10. Google Search Console (optional but recommended)

> 📖 Poora guide (verification, sitemap submit, request indexing, Bing, IndexNow) →
> **[Part 6 — Search Console](Step6-SearchConsole.md)** — Step 7

1. https://search.google.com/search-console
2. **Add property** → Domain type (`yourdomain.com`)
3. DNS se verify karo (Cloudflare pe TXT record auto add ho jata hai)
4. **Sitemaps** → `sitemap.xml` submit karo
5. **URL Inspection** → `https://yourdomain.com` → **Request indexing**

> Ab se Google tumhari site ko crawl karega. Step 1-2 ke traffic ke liye zaroori hai.

---

## ⚠️ Troubleshooting

| Problem | Fix |
|---------|-----|
| **522** Connection timed out | Domain Pages dashboard mein add nahi hua, ya nameservers galat. CNAME manually check karo |
| **525** SSL handshake failed | SSL mode **Full (Strict)** karo |
| **Certificate pending** | Nameserver propagation wait karo (24 hrs tak) |
| **Site 404** | Build output / root directory galat — `dist` ya `sachindev.com` folder sahi hai? |
| **Domain kharida par site nahi dikhti** | Registrar mein nameservers save nahi hue → phir se set karo |
| **Email nahi aa raha** | MX records check karo → Email Routing → Records |
| **www nahi chal raha** | CNAME record `www` → `<project>.pages.dev` banao |
| **Changes live nahi ho rahe** | `npm run deploy` chalaya? Git integration wale projects mein auto-deploy hota hai |

---

## ✅ Cloudflare Checklist

- [ ] Cloudflare account bana (Free plan)
- [ ] Site added + DNS records review kiye (delete nahi kiye purane)
- [ ] `npm run deploy` — site `*.pages.dev` / `*.workers.dev` pe live
- [ ] Domain kharida (`.com`, renewal price check kiya, auto-renew ON, 2FA ON)
- [ ] Cloudflare nameservers registrar mein set hain (`nslookup` se verify)
- [ ] Domain Pages/Workers project se connected hai
- [ ] `www` ka CNAME bana + **301 redirect** (www ↔ apex, ek hi canonical URL)
- [ ] `astro.config.mjs` me `site: 'https://yourdomain.com'` set hai
- [ ] SSL **Active** + `https://yourdomain.com` chal raha hai
- [ ] `workers.dev` / `pages.dev` indexing band (noindex / disable)
- [ ] Personal email set — `admin@yourdomain.com` → gmail (Routing, free) + destination verified
- [ ] Search Console mein property added + sitemap submitted
- [ ] 🎉 **Site live on your own domain — $0 cost**

---

## 🔗 Saare Links (quick reference)

### Domain
| Kaam | Link |
|------|------|
| Availability check | https://instantdomainsearch.com/ |
| WHOIS / expired domain check | https://lookup.icann.org/ · https://www.whois.com/ |
| Domain register — Hostinger ⭐ | https://www.hostinger.com/in |
| Domain search — Hostinger | https://www.hostinger.com/in/domain-name-search |
| Domain register — Namecheap | https://namecheap.pxf.io/c/7622160/386170/5618 |
| Domain register — Spaceship | https://spaceship.sjv.io/c/7622160/1794549/21274 |
| Domain deals — Spaceship promos 💰 | https://www.spaceship.com/promos/ |
| Domain register — BigRock | https://www.bigrock.in/ |
| Domain register — GoDaddy | https://www.godaddy.com/en-in |
| Cloudflare Registrar (at-cost) | https://www.cloudflare.com/products/registrar/ |

### Cloudflare
| Kaam | Link |
|------|------|
| Dashboard | https://dash.cloudflare.com/ |
| Add a site | https://dash.cloudflare.com/?to=/:account/add-site |
| Workers & Pages | https://dash.cloudflare.com/?to=/:account/workers-and-pages |
| Nameservers setup | https://developers.cloudflare.com/dns/zone-setups/full-setup/setup/ |
| Pages — custom domains | https://developers.cloudflare.com/pages/configuration/custom-domains/ |
| Workers — custom domains | https://developers.cloudflare.com/workers/configuration/routing/custom-domains/ |
| Workers — static assets | https://developers.cloudflare.com/workers/static-assets/ |
| Workers — limits (100k/day) | https://developers.cloudflare.com/workers/platform/limits/ |
| Pages — limits (500 builds) | https://developers.cloudflare.com/pages/platform/limits/ |
| SSL / TLS | https://developers.cloudflare.com/ssl/ |
| Email Routing (inbound, free) | https://developers.cloudflare.com/email-service/get-started/route-emails/ |
| Email Sending (outbound, Beta) | https://developers.cloudflare.com/email-service/get-started/send-emails/ |
| Email Service — overview | https://developers.cloudflare.com/email-service/ |
| Email — limits | https://developers.cloudflare.com/email-service/platform/limits/ |
| Email — pricing | https://developers.cloudflare.com/email-service/platform/pricing/ |
| Email — deliverability | https://developers.cloudflare.com/email-service/concepts/deliverability/ |
| `_headers` file rules | https://developers.cloudflare.com/pages/configuration/headers/ |
| Redirects (overview + limits) | https://developers.cloudflare.com/rules/url-forwarding/ |
| Single Redirects (www → apex) | https://developers.cloudflare.com/rules/url-forwarding/single-redirects/ |
| Bulk Redirects (pages.dev → domain) | https://developers.cloudflare.com/rules/url-forwarding/bulk-redirects/ |
| DNS proxy status (orange cloud) | https://developers.cloudflare.com/dns/proxy-status/ |
| Rules simulator (test redirect) | https://developers.cloudflare.com/rules/simulator/ |
| Deploy prompts (AI ke liye) | [Part 4 — Deploy](Step4-Deploy.md) |

### SEO / Growth
| Kaam | Link |
|------|------|
| Google Search Console | https://search.google.com/search-console |
| PageSpeed Insights | https://pagespeed.web.dev/ |
| Google PageSpeed (API) | https://developer.chrome.com/docs/lighthouse/overview/ |

---

## Deliverable

Your live website on your own `.com` domain — free SSL, Cloudflare global CDN, free email
forwarding, auto-deploy on every commit, **$0 hosting cost**.

**Next:** [Step 7 — Analysis & Webmaster Tools](Step6-SearchConsole.md) (Part 6)
