# Part 7 — Earnings

> **Traffic in. Money out.**
>
> AdSense Approval (real content + policy pages) · Common Rejects (thin content, low value) ·
> Live Ads (placement without killing UX) · Repeat × 12 (next tool, next month)

**Part 7 — Earnings**

---

## Earning (Google AdSense)

**Goal:** Apni site se paise kamana — AdSense approval → ads live → payment.

**Apply link:** https://adsense.google.com/start/

| Card | Kya hai |
|------|---------|
| **AdSense Approval** | real content + policy pages |
| **Common Rejects** | thin content, low value |
| **Live Ads** | placement without killing UX |
| **Repeat × 12** | next tool, next month |

---

## 1. Kab apply karna hai? ⏰

| Milestone | Detail |
|-----------|--------|
| **10+ daily users** | GA4 Real-time + Search Console se confirm karo — **tab apply karo** |
| Site live | Apne `.com` domain pe (❗ `pages.dev` subdomain pe AdSense allow nahi hota) |
| Sabhi pages ready | [Part 3](Step3-Build.md) ke saare pages — Privacy, Terms, About, Contact, 404, robots.txt, sitemap.xml |
| Content ready | 600-word SEO content + FAQ (JSON-LD) already lagaya hua |
| UX clean | Mobile responsive, fast, dark mode, ads ki jagah bani hui |
| Domain age | Kuch hafte se chal raha ho (community guidance: **~6 mahine** ideal) |

> 💡 **Traffic se zyada content matter karta hai.** 10 daily users + 20+ quality pages =
> strong application. 100 daily users + 5 pages = reject.

**Traffic check kaise karein:**
- GA4 → Reports → **Realtime** (abhi kitne log hain)
- GA4 → Reports → **Engagement** (daily users)
- Search Console → **Performance** → last 7 days clicks

---

## 2. Sign up (https://adsense.google.com/start/)

1. https://adsense.google.com/start/ → **Get started**
2. **Google account** se sign in (wahi account jo Search Console/Analytics me hai — same hi rakho)
3. **Add site** → apna `.com` domain daalo (`yourdomain.com`)
   - AdSense tumhare domain ko **DNS se verify** karta hai (TXT record / ADNS)
   - Cloudflare → DNS → Records → **TXT** record add karo (grey cloud) → Save
4. **AdSense for Web** choose karo (free) — "Video" / "Games" mat chuno
5. Personal info + **payment** details:
   - India: **PAN** + bank account (IFSC)
   - Payout threshold: **$100** minimum
6. Submit → **review** shuru (usually **1–2 weeks**, kabhi 4 weeks+)

> 🔁 **Rejected mile toh:** AdSense dashboard me reason dikh jayega. Reason padho → fix karo →
> **Re-apply** (30 din tak wait karna padta hai).

---

## 3. `ads.txt` — authorized sellers file (recommended)

Ye file batata hai kaun kaun si ad networks tumhare site pe ads chha sakte hain. Isse
**unauthorized sellers** block hote hain (unauthorized ads = policy violation).

**1. Publisher ID copy karo:** AdSense → **Account** → publisher ID (`pub-XXXXXXXXXXXXXXXX`)

**2. File banao:** project root me `public/ads.txt`

```
google.com, pub-0000000000000000, f08c47fec0942fa0, DIRECT
```

> `pub-0000000000000000` → apna **real publisher ID** daalo. Baaki exactly waisa hi.

**3. Deploy + verify:**
```bash
npm run deploy
```
Check: `https://yourdomain.com/ads.txt` → file dikhni chahiye ✅
Aur dashboard me: https://adsense.google.com/adsense/myads/ads.txt

> 💡 `ads.txt` mandatory nahi hai, lekin **zalmi karo** — fake advertisers se bachata hai.
> AI se:
> ```text
> Create a `public/ads.txt` file for my AdSense account with my publisher ID
> pub-XXXXXXXXXXXXXXXX and the Google AdSense line.
> ```

---

## 4. Live Ads — placement (UX mat kharab karo!)

### Pehle: Auto ads (beginners ke liye best)

1. AdSense → **Ad settings** → **Auto ads** → **ON**
2. Script code copy karo → `BaseLayout.astro` ke `<head>` me (ek hi baar):

```html
<script async src="https://pagead2.googlesyndication.com/pagead/js/adsbygoogle.js?client=ca-pub-XXXXXXXXXXXXXXXX"
        crossorigin="anonymous"></script>
```

AI se:
```text
Add Google AdSense auto ads to this Astro site using my publisher ID
ca-pub-XXXXXXXXXXXXXXXX. Load the script once in the BaseLayout head with async
and crossorigin="anonymous". Don't add it twice.
```

### Manually ad unit lagana hai

AI se ek reusable component banwao:

```text
Create a reusable AdSlot.astro component for Google AdSense. It takes an adSlotId prop,
renders the <ins class="adsbygoogle"> markup, and pushes to the adsbygoogle queue.
Use my publisher ID ca-pub-XXXXXXXXXXXXXXXX. I will place it below the fold.
```

**Kahan rakhna hai (safe spots):**

| Position | Safe? |
|----------|-------|
| Tool ke **neeche** (result ke baad) | ✅ Best |
| Content ke end me | ✅ Best |
| Sidebar / footer | ✅ Safe |
| Header ke upar / hero ke andar | ⚠️ Risky (accidental clicks) |
| Tool ke buttons ke paas | ❌ Mat karo |
| Pop-up / pop-under | ❌ Policy violation |
| Login ke peeche | ❌ Google ads nahi dikha sakta |

### ⚠️ Official placement rules (policy violation se bacho)

Source: https://support.google.com/adsense/answer/1346295

- ❌ Ads ko link/button jaisa mat dikhao (accidental clicks)
- ❌ Ads pe flash/arrow/animation mat lagao ("unnatural attention")
- ❌ Ads ko "Resources" / "Sponsored Links" jaise misleading heading ke neeche mat rakho
  - ✅ Sirf **"Advertisement"** ya **"Sponsored Links"** likhna allowed hai
- ❌ Page auto-refresh mat karo
- ❌ Pop-up/pop-under me ads mat rakho (max 3 popups, pop-under bilkul nahi)
- ❌ Ads ko `display:none` se chhupaao (responsive ad unit ke alawa)
- ❌ Ads ko content ke upar slide/zoom mat karo
- ❌ "Click our ads" jaisa text mat likho
- ✅ AdSense code ko copy-paste hi rakho (thoda modification ok hai, but Artificial inflation nahi)

Aur: https://support.google.com/adsense/answer/1354736 (code modifications)

---

## 5. Common Rejects + fix ⛔

| Reject reason | Kya hota hai | Fix |
|---------------|---------------|-----|
| **Low value content** | Pages khali / bahut kam text | Har page pe **real content** — 600+ words, images, how-to |
| **Thin content** | Sirf tool, text nahi | Tool ke saath **guide/how-to** section, FAQ, examples |
| **Navigation pages** | Sirf About/Contact/Privacy | **20+ quality pages** chahiye (community guidance) |
| **Privacy policy missing** | Page hi nahi | [Part 3](Step3-Build.md) ka Privacy Policy page ready karo |
| **Site under construction** | Half-finished feel | Sab pages ka content, working links, working tool |
| **Low traffic** | Koi nahi aata raha | 10+ daily users tak traffic laao (SEO + IndexNow) |
| **Copied content** | Kahin se copy | Original content likho (AI se draft + apne words) |
| **Ad placement issues** | Ads galat jagah | Upar wale placement rules follow karo |
| **Domain not eligible** | `pages.dev` / subdomain | Apna `.com` domain use karo |
| **Site is not ready** | Half-ready | Review se pehle poora checklist complete karo |

---

## 6. Extra pages jo AdSense pasand karta hai

- **"About these ads"** page — ads kaise aate hain, ye explain karo (users ka trust badhta hai)
- **Author / Editorial policy** page — kisne likha, kaise likha
- **Contact** page with real form + email (Cloudflare Email Routing se — [Part 5](Step5-Cloudfare.md) Section 9)
- **Cookie / consent banner** (EEA traffic ho toh zaroori)

---

## 7. Earning badhao 💰

| Tip | Detail |
|-----|--------|
| **Auto ads ON** | Best for beginners — Google khud best placement dhoondhta hai |
| **Placement > CTR** | Sahi jagah ad = zyada click (aur policy safe) |
| **USA/UK traffic = high RPM** | ₹ me convert karne se pehle **geo traffic** dekho (GA4) |
| **Content + ads** | Ads se pehle **value** do — phir ads |
| **ads.txt** | Unauthorized sellers block ✅ |
| **Blocklist** | AdSense → **Blocklists** → categories block karo (gambling etc.) |
| **A/B test** | AdSense → **Experiments** (free) — best layout test karo |
| **Privacy-friendly analytics** | GA4 ke saath **Cloudflare Web Analytics** bhi (free, no cookie) |

---

## 8. How Long Until You Start Earning? ⏳

| Time | Kya hoga |
|------|----------|
| **Month 1–3** | Website is live. Google is crawling it. Rankings are low. Keep building more tools. |
| **Month 4–6** | Rankings begin to climb if your SEO and content are solid. Usually **4–6 months to first meaningful traffic**. |
| **Month 6+** | With consistent traffic (**10+ daily users**), apply for AdSense. First earnings appear. |
| **Year 1** | With **12 tools live**, 1–3 will likely be driving consistent organic traffic. Passive income has begun. |
| **Year 2** | **24 tools.** Compounding effect. The best tools keep growing. AdSense revenue scales with traffic. |

> 💡 **Expectation set:** AdSense se pehle koi earning nahi hoti. Pehla paisa aata hai
> jab traffic aaye — 4–6 months. Isliye **ek tool pe rukna mat**, har month next tool banao
> (Section 9 — Repeat × 12).

---

## 9. Repeat × 12 — next tool, next month 🔁

**Strategy:** 1 tool → 1 domain → 1 site → 1 month me next tool.

```
Month 1  →  Tool 1 (sachindev.com)      → AdSense → traffic → ₹
Month 2  →  Tool 2 (tool2.com)          → AdSense → traffic → ₹
...
Month 12 →  Tool 12 (tool12.com)        → AdSense → traffic → ₹
```

Har tool ke liye same 7-step cycle (yehi guide):
**Find Problem → Verify → Competitors → Golden Keyword → Domain → Build → Deploy**

> 💡 Isse har month **ek naya earning source** banta hai. Ek site se zyada expect mat karo —
> 10 daily users + sahi ads = stable income. 12 sites = 12 income streams.

**Har tool ke liye AdSense alag?** Nahi — **ek AdSense account me multiple sites** add kar
sakte ho (Account → Sites → Add site). Har site ka apna ads.txt + policy pages.

---

## 10. AdSense reject ho gaya? Alternatives (for backup)

| Network | Link | Best for |
|---------|------|----------|
| **Media.net** | https://www.media.net/ | Content sites, high RPM |
| **Ezoic** | https://www.ezoic.com/ | AI ad placement + revenue optimizer |
| **Monetag** | https://www.monetag.com/ | Small traffic sites |
| **Adsterra** | https://www.adsterra.com/ | Small traffic, quick approval |
| **PropellerAds** | https://propellerads.com/ | Social / push ads |
| **Google AdSense for YouTube** | https://support.google.com/youtube/answer/72851 | Video content |
| **Google Ad Manager** | https://admanager.google.com/home/ | Big publishers, direct ads |

> ⚠️ Inme bhi **same policy mind-set** chahiye — genuine content, no forced clicks.
> AdSense best RPM deta hai — alternatives **backup** ke liye rakho.

---

## ✅ Earning Checklist

- [ ] GA4 me **10+ daily users** dikh rahe hain
- [ ] Site apne `.com` domain pe live hai (❌ pages.dev nahi)
- [ ] Sabhi policy pages hain: **Privacy Policy, Terms, About Us, Contact Us**
- [ ] 404 page, robots.txt, sitemap.xml ready
- [ ] 20+ pages with real content (600+ words each + how-to)
- [ ] `public/ads.txt` bana + deploy + verify kiya
- [ ] AdSense apply kiya (https://adsense.google.com/start/)
- [ ] DNS TXT record se domain verify kiya
- [ ] Payment + tax info (PAN + bank) bhar di
- [ ] Approval mila → Auto ads ON + script `BaseLayout` head me
- [ ] Ads **sirf safe jagah** lage (tool ke neeche / content ke end me)
- [ ] AdSense **Review** → status check kiya
- [ ] Reject hua → reason padha + fix + re-apply
- [ ] **Next tool plan** (Repeat × 12) — month 2 ka domain shortlist karo

---

## 🔗 Saare Links (quick reference)

| Kaam | Link |
|------|------|
| **AdSense apply** | https://adsense.google.com/start/ |
| AdSense login / dashboard | https://adsense.google.com/adsense/myads |
| **ads.txt (dashboard)** | https://adsense.google.com/adsense/myads/ads.txt |
| AdSense help center | https://support.google.com/adsense/ |
| Program policies | https://support.google.com/adsense/answer/48182 |
| Ad placement policies | https://support.google.com/adsense/answer/1346295 |
| Ad code modifications | https://support.google.com/adsense/answer/1354736 |
| Responsive ad units | https://support.google.com/adsense/answer/9183362 |
| AdSense Experiments (A/B) | https://support.google.com/adsense/answer/6321879 |
| AdSense for Shopping | https://support.google.com/adsense/answer/6078274 |
| Optimization tips (personalized) | https://support.google.com/adsense/answer/10568458 |
| AdSense blog | https://blog.google/products/adsense/ |
| AdSense API (developers) | https://developers.google.com/adsense/management/ |
| Site Kit (WordPress only) | https://sitekit.withgoogle.com/ |

---

## Deliverable

**Approved AdSense account** with live ads on your site, `ads.txt` verified, payment details
set — and a **12-month plan** of one new tool every month.

**Next:** [Part 8 — Backlinks](Step8-Backlinks.md)
