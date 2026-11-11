# Part 6 — Search Console

> **Analysis & Webmaster tools.** Google Analytics · Search Console · Bing Webmaster Tools ·
> IndexNow · Sitemaps · Request Indexing

**Step in this part:** Step 7

---

## Step 7 — Analysis & Webmaster Tools

**Goal:** Site ka data dekhna (Analytics), Google ko batana ki site exist karti hai
(Search Console + sitemap + indexing), aur Bing/Emoji search engines ko bhi.

| Tool | Kaam | Cost |
|------|------|------|
| **Google Analytics 4** | Kitne log aaye, kahan se aaye, kya kar rahe hain | Free |
| **Google Search Console** | Google ko batana ki site live hai → indexing | Free |
| **Bing Webmaster Tools** | Bing/SEO data + instant indexing (IndexNow) | Free |
| **IndexNow** | Naya page turant index karwana | Free |

---

## 1. Google Analytics 4 (GA4) — data dekhna

**Main link:** https://analytics.google.com/analytics/web/
**About:** https://support.google.com/analytics/answer/9304153

### Setup steps

1. https://analytics.google.com/analytics/web/ kholo → **Google account** se sign in karo
2. **Admin** (sidebar ka neeche gear) → **Create property**
3. Form bhare:
   - **Property name**: `yourdomain.com`
   - **Website URL**: `https://yourdomain.com`
   - **Industry category**: apni category
   - **Timezone** + **Currency**
4. **Create** → **Measurement ID** milega: `G-XXXXXXXXXX`
5. ID ko site me lagao — [Part 3 — Build](Step3-Build.md) me GA4 section hai (gtag.js snippet
   `BaseLayout.astro` ke head me) → ya AI se:
   ```text
   Add Google Analytics 4 to this Astro site using my measurement ID G-XXXXXXXXXX.
   Put the gtag.js script in the shared BaseLayout head so it loads on every page,
   async, and don't inject it twice.
   ```
6. Deploy karo → `npm run deploy`

### Verify + reports

| Kahan | Link |
|-------|------|
| Property overview | https://analytics.google.com/analytics/web/ |
| Reports (Traffic, Engagement) | Reports → **Reports snapshot** |
| **Real-time** (abhi kaun hai site pe) | Reports → **Realtime** |
| **Intelligent events** | Reports → **Intelligent events** |
| Debug / check tag | Admin → **Data Streams** → Web → status + **Test** |

> ⏱️ Data aane me **24–48 ghante** lag sakte hain. Real-time me turant dikhta hai.

### AdSense ke liye zaroori (if monetization)

GA4 aur AdSense ko link karo — AdSense approval me madad milti hai:

- GA4 → **Admin** → **AdSense linking** → Connect
- AdSense → Admin → **Account summary** → link your Analytics property

---

## 2. Google Search Console — site ownership verify (Cloudflare DNS record)

**Main link:** https://search.google.com/search-console/about
**Console:** https://search.google.com/search-console

### Step A — Property add karo

1. https://search.google.com/search-console → sign in (Google account)
2. **Add property** → **Domain** type choose karo → `yourdomain.com` daalo
   - ⚠️ **Domain** type sabse strong hai (covers http/https + www/non-www)
3. Google ek **TXT verification record** dega:
   ```
   google-site-verification=AbCdEf1234567890...
   ```

### Step B — Cloudflare me record add karo (DNS record method)

1. Cloudflare dashboard → **Websites** → `yourdomain.com` → **DNS** → **Records**
2. **Add record**:
   | Type | Name | Content | Proxy | TTL |
   |------|------|---------|-------|-----|
   | **TXT** | `@` | `google-site-verification=AbCdEf1234567890...` | **DNS only** (grey cloud ☁️) | Auto |
3. **Save**
4. GSC wapas jao → **Verify**

> ⚠️ **Proxy OFF (grey cloud) zaroori hai** — TXT records proxied nahi ho sakte.
> ⏱️ Verification **48 ghante** tak lag sakta hai (usually 5–30 min).

### Alternative verification methods

| Method | Kaise |
|--------|-------|
| **DNS TXT** (recommended) | Upar wala ✅ |
| **HTML file** | `google<random>.html` file site root me upload karo (`public/` me daal ke deploy karo) |
| **Meta tag** | `<meta name="google-site-verification" content="...">` head me |
| **URL prefix** | `https://yourdomain.com/` ya `https://www.yourdomain.com/` prefix ke saath property |

---

## 3. Sitemap upload / submit

> ⚠️ Pehle sitemap **banani** padegi → [Part 3 — Build](Step3-Build.md) me
> "🗺️ sitemap.xml" section hai (`@astrojs/sitemap` + `site` config).

### Google Search Console

1. https://search.google.com/search-console → apna domain select karo
2. Left menu → **Sitemaps**
3. **Sitemap** box me likho:
   ```
   sitemap.xml
   ```
   (ya full URL: `https://yourdomain.com/sitemap.xml`)
4. **Submit** → status **Success** dikhna chahiye

> 💡 `robots.txt` me bhi sitemap link hona chahiye:
> ```
> Sitemap: https://yourdomain.com/sitemap.xml
> ```
> Isse Google ko sitemap mil jata hai automatically.

### Bing Webmaster Tools

1. https://www.bing.com/webmasters/ → sign in
2. **Sitemaps** → **Submit Sitemap** → `https://yourdomain.com/sitemap.xml` → **Submit**
3. Ya GSC se import: **Sitemaps** → **Import Sitemaps from GSC** (one click)

---

## 4. URL Inspection → Request Indexing (Google)

Naya page Google ko turbat chahiye toh manually request karo.

1. https://search.google.com/search-console → top bar me **URL daalo** → **Enter**
   ```
   https://yourdomain.com/your-new-page
   ```
2. Result me:
   - ✅ **URL is on Google** → done, kuch nahi karna
   - ⚠️ **URL is not on Google** → pehle **TEST LIVE URL** dabao (page live hai ya nahi check)
3. **REQUEST INDEXING** dabao → request queue me lag jayegi (1–2 din me crawl hoti hai)

> 💡 **Quota limited hai** — sab pages pe mat dabao. Sirf important pages (home, main tool,
> key landing pages) pe use karo. Baaki sitemap se crawl ho jayenge.

### Bulk me kaam karna hai?

- **Sitemaps** se hi bulk indexing hoti hai — har naya page sitemap me aa jaye
- Ya **IndexNow** (neeche) — **instant**, 10,000 URLs ek request me

---

## 5. Bing Webmaster Tools (Google se data le sakta hai!)

**Main link:** https://www.bing.com/webmasters/about
**Console:** https://www.bing.com/webmasters/

### Setup — sabse aasan tarika: GSC se import

1. https://www.bing.com/webmasters/ → **Microsoft account** se sign in (ya Google se bhi chal jata hai)
2. **Import from Google Search Console** → apna GSC account connect karo
3. Apni site dikh jayegi → **Import** dabao
   - ⚠️ **Bing GSC ka data khud pull kar leta hai** — traffic, keywords, impressions sab aa jate hain.
   Mana kiya toh manual add kar sakte ho (neeche)

### Manual add + verify (Cloudflare DNS method)

1. **Add a site** → `yourdomain.com`
2. Verification method choose karo:
   | Method | Kya karna hai |
   |--------|---------------|
   | **Import from GSC** | Sabse aasan ✅ |
   | **CNAME record** | DNS me `bingverify.<random>` → CNAME → `bing.com` |
   | **TXT record** | DNS me Name `@`, Content `msvalidate.01=<code>`, **DNS only** (grey cloud) |
   | **Meta tag** | `<meta name="msvalidate.01" content="<code>" />` head me |
   | **HTML file** | `BingSiteAuth.xml` site root me upload karo |

3. Cloudflare → **DNS** → **Add record** → **TXT** (ya CNAME) → **DNS only** → **Save**
4. Bing wapas → **Verify**

### Sitemap + URL inspection (Bing)

- **Sitemaps** → **Submit Sitemap** → `https://yourdomain.com/sitemap.xml` → Submit
- Top bar me URL daalo → **URL Inspection** → index status dekho → **Request Indexing**

### Bing ke extra free tools

| Tool | Kaam |
|------|------|
| **Site Explorer** | Indexed pages, backlinks, keywords — sab free |
| **Performance** | Queries + clicks + impressions (GSC jaisa, free) |
| **Crawl Control** | Googlebot ko slow/allow karo |
| **IndexNow** | Instant indexing (neeche) |
| **URL Submission** | Limited quota ke saath turant submit |

---

## 6. IndexNow — instant indexing (free)

**Docs:** https://www.indexnow.org/documentation

IndexNow protocol: **content badla → turant search engines ko bata do** → minutes me crawl.

1. **Bing Webmaster Tools** → **IndexNow** tab → **Create key** (8–128 hex characters)
2. Key file site root pe host karo — Astro me `public/` me daal do:
   ```
   public/1a2b3c4d5e6f7a8b.txt   ← file ke andar sirf wahi key likho
   ```
   → `https://yourdomain.com/1a2b3c4d5e6f7a8b.txt`
3. AI se setup karwao:
   ```text
   Add IndexNow support to this Astro site. Use key 1a2b3c4d5e6f7a8b, host the key file
   in public/, and send a POST request to the Bing IndexNow endpoint whenever a page is
   created or updated (e.g. after build in an Astro hook or on deploy).
   ```
4. URL submit karo (one URL):
   ```bash
   curl "https://api.indexnow.org/indexnow?url=https://yourdomain.com/new-page&key=1a2b3c4d5e6f7a8b"
   ```
5. Bulk (10,000 URLs max, ek request):
   ```bash
   curl -X POST "https://api.indexnow.org/indexnow" \
     -H "Content-Type: application/json" \
     -d '{
       "host": "yourdomain.com",
       "key": "1a2b3c4d5e6f7a8b",
       "urlList": [
         "https://yourdomain.com/page1",
         "https://yourdomain.com/page2"
       ]
     }'
   ```

> 💡 **IndexNow sirf Bing tak nahi** — protocol follow karne wale sab search engines
> (Bing, Yandex, Naver, Seznam…) ko bhi URL mil jata hai.
> ⚠️ Zyada spam mat karo — **429 Too Many Requests** milta hai. Bulk me ek request bhejo.

---

## 7. Extra (free) webmaster tools

| Tool | Link | Kaam |
|------|------|------|
| Google PageSpeed Insights | https://pagespeed.web.dev/ | Speed score + Core Web Vitals |
| Cloudflare Web Analytics | https://dash.cloudflare.com/ | Free, cookie-less, no script needed |
| Ahrefs Webmaster Tools | https://ahrefs.com/webmaster-tools/ | Free, GSC se connect |
| Bing Webmaster Tools | https://www.bing.com/webmasters/ | Keywords + IndexNow |
| W3C Markup Validator | https://validator.w3.org/nu/ | HTML errors |
| Rich Results Test | https://search.google.com/test/rich-results | Schema/FAQ check |

---

## ✅ Analysis Checklist

- [ ] GA4 property banayi + measurement ID `G-XXXXXXXXXX` site pe lagaya (Part 3)
- [ ] GA4 me **Real-time** me visitors dikh rahe hain
- [ ] AdSense ↔ GA4 linked (agar monetization karna hai)
- [ ] Search Console me **Domain property** add kiya
- [ ] Cloudflare DNS me **TXT record** (grey cloud) add karke GSC verify kiya
- [ ] `sitemap.xml` generate hua (Part 3) + GSC me **submit** kiya (Success ✅)
- [ ] `robots.txt` me `Sitemap:` line hai
- [ ] Important pages ka **URL Inspection → TEST LIVE URL → REQUEST INDEXING** kiya
- [ ] Bing Webmaster Tools me **Import from GSC** (ya manual verify) kiya
- [ ] Bing me bhi **sitemap submit** kiya
- [ ] IndexNow key banayi + `public/` me key file rakhi + URL submit kiya
- [ ] PageSpeed Insights me green score ✅

---

## 🔗 Saare Links (quick reference)

| Kaam | Link |
|------|------|
| Google Analytics 4 | https://analytics.google.com/analytics/web/ |
| GA4 setup guide | https://support.google.com/analytics/answer/9304153 |
| GA4 → AdSense linking | https://support.google.com/analytics/answer/13610380 |
| Google Search Console (about) | https://search.google.com/search-console/about |
| Google Search Console | https://search.google.com/search-console |
| Sitemap submit (GSC) | https://search.google.com/search-console/sitemaps |
| URL Inspection (GSC) | https://search.google.com/search-console |
| Bing Webmaster Tools (about) | https://www.bing.com/webmasters/about |
| Bing Webmaster Tools | https://www.bing.com/webmasters/ |
| Bing — add & verify site | https://www.bing.com/webmasters/help/add-and-verify-site-12184f8b |
| Bing — import from GSC | https://www.bing.com/webmasters/help/import-from-gsc-12184f8b |
| Bing — add sitemap | https://www.bing.com/webmasters/help/add-sitemap-12184f8b |
| Bing — URL inspection | https://www.bing.com/webmasters/help/url-inspection-12184f8b |
| Bing — URL submission | https://www.bing.com/webmasters/help/url-submission-12184f8b |
| IndexNow docs | https://www.indexnow.org/documentation |
| PageSpeed Insights | https://pagespeed.web.dev/ |
| Rich Results Test | https://search.google.com/test/rich-results |

---

## Deliverable

Your site **fully indexed** — Google Analytics me live traffic dikh raha hai, Google Search
Console me verified + sitemap submitted, Bing me imported + IndexNow se instant indexing,
aur har page ki URL inspection me "on Google" ✅.

**Next:** [Step 8 — Earning (Google AdSense)](Step7-Earnings.md) (Part 7)
