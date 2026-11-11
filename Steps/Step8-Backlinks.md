# Part 8 — Backlinks

> **Benefits.** Higher Rankings · Referral Traffic · Faster Indexing · Brand Authority
>
> Apne backlinks banao, track karo, aur **interlink slowly** — 1 link edit per month.

**Part in this guide:** 8

---

## Backlinks & Interlinking

**Goal:** Apne hi sites se dusri sites ko link karke (interlinking) rankings, referral
traffic, indexing speed aur brand authority badhana — bina kisi paid link ke.

---

## ✨ Benefits

| Benefit | Kyun |
|---------|------|
| **Higher Rankings** | Google sees credibility — doosri sites ka link tumhari site ko "vote" deta hai |
| **Referral Traffic** | Real humans click through — users seedha tumhare tool pe aate hain |
| **Faster Indexing** | Bots naye pages ko jaldi crawl karte hain jab links milte hain |
| **Brand Authority** | Long-term trust — jab 5-10 sites ek dusre ko point karein, brand banta hai |

> 💡 **Interlinking ka sach:** Ye "link scheme" nahi hai (Google ke rules ke andar hai) —
> kyunki ye **tumhari apni content ke beech** natural links hain. Google isko
> "internal linking" maanta hai jo bahut helpful hai. Lekin **ek hi mahine me 100 links
> nahi** — slow & natural raho (neeche strategy).

---

## 1. Backlinks kya hote hain? (Types — detail list)

### A. Attribute wise

| Type | Kya hai | Value |
|------|---------|-------|
| **Do-follow** | `rel="dofollow"` — link juice pass hota hai | 🟢 Sabse strong |
| **No-follow** | `rel="nofollow"` — juice nahi jaata, but signal banta hai | 🟡 Natural mix mein best |
| **Sponsored** | `rel="sponsored"` — paid/affiliate link | 🟡 Tab use karo jab paid ho |
| **UGC** | User Generated Content — Reddit/Quora/forum comment | 🟢 Real traffic + link |
| **Profile links** | GitHub, Medium, LinkedIn, Crunchbase profile | 🟡 Low value, easy |
| **Sidebar/footer links** | Pure website link | 🔴 Low value |

### B. Source wise (jahan se link aata hai)

| Type | Example | Note |
|------|---------|------|
| **Editorial** | News article, blog review | 🟢 Sabse natural |
| **Resource page** | "Best free tools" list | 🟢 Bahut strong |
| **Guest post** | Dusre blog me article likha | 🟢 Natural + referral |
| **Broken link building** | Dead link mila → apna content suggest kiya | 🟢 Easy win |
| **Skyscraper** | Sade content ko better bana ke link maanga | 🟢 High authority |
| **Reclaim / unlinked mention** | Brand mention hai, link nahi → request | 🟢 Sabse easy |
| **Directory / listing** | Directory submit | 🔴 Low value (spam mat karo) |
| **Forum / community** | Reddit, Quora, niche forum | 🟢 UGC |
| **Comment links** | Blog comments | 🔴 Usually nofollow + low value |
| **Press release** | Newswire | 🔴 Google ignore karta hai |
| **Social media** | X, LinkedIn, Pinterest bio | 🟡 No-follow, but reach |
| **Image / video link** | Infographic, YouTube description | 🟡 Good for reach |
| **PDF / document links** | Slideshare, Scribd, Docs | 🟡 Works |
| **Email / newsletter** | Mention + link | 🟢 Real traffic |
| **Paid links** | Guest post marketplace, PBN | 🔴 **Risk** — Google penalize karta hai |
| **Affiliate link** | Product review with affiliate link | 🟡 `sponsored` use karo |

### C. Self-made vs earned

| | Description |
|---|---|
| **Earned (natural)** | Kisi ne apne aap link kiya — sabse safe + powerful |
| **Self-made** | Tumne khud banaya — interlinking, resource pages, directories |
| **Paid** | Paise diye — **Google ke link spam policies ke against** ❌ |

> ⚠️ Google ki **link spam policy** padho: https://developers.google.com/search/docs/essentials/spam-policies#link-spam
> Buying links, link farms, PBNs, mass guest posts, low-quality directories, spun content —
> sab penalty risk hai. **Interlinking apni sites ka safe hai, kyunki woh content ke andar hai.**

---

## 2. Apne backlinks kaise banayein (guide)

### Sabse pehle — apne hi tools ko link karo (FREE + safe)

1. Har site me ek **"Related tools"** section banao (footer ya content ke end me)
2. Har tool me **2-3** dusre tools ke links do (contextual — content ke andar)
3. Link text **exact match** keyword rakho (e.g., "online ruler" → `/online-ruler`)
4. `rel="nofollow"` mat lagao apni sites ke beech (nahi toh link juice nahi jaayega)

```html
<!-- example: content ke andar natural link -->
<p>Need a quick measurement? Try our <a href="https://site2.com/online-ruler">online ruler</a>
— it works on mobile too.</p>
```

### AI prompt (apni site me link section banane ke liye)

```text
Add a "Related tools" section to this website. Create a data file `src/data/related.ts`
with an array of { title, url, description } objects for my other tools, and render it
after the main content and before the footer on every page. Use plain <a> tags with
descriptive anchor text matching each tool's keyword, no nofollow, and style it as a
simple responsive grid that works on mobile.
```

### Bade sources se link (hard mode, but real traffic)

| Kaise | Steps |
|-------|-------|
| **Resource page outreach** | "Best free X tools" lists dhundho → owner ko email/tool suggest karo |
| **Broken link building** | Purane articles me dead links dhundho → apna content suggest karo |
| **Guest post** | Same niche ke blogs ko pitch karo (free tools wale blogs best) |
| **Community answers** | Reddit/Quora pe genuinely help karo + link (spam mat karo) |
| **Reclaim mentions** | Brand mention mila, link nahi → polite email |
| **Free tools directories** | 5-10 legit directories me submit (no paid bulk submit) |

### AI prompt (outreach emails)

```text
Write 5 short outreach emails for getting a backlink from relevant blog/resource pages.
Each email should be under 120 words, mention a specific page on their site that I could
contribute to (a mini guide, a tool, or a data point), and give a clear one-step ask.
Tone: helpful expert, not salesy. No follow-up spam.
```

---

## 3. 🔗 Interlink. Slowly. (the strategy)

> **Rule: only 1 link edit per month.** Har site ko ek hi baar me link mat karo —
> slowly, natural, one-at-a-time.

### Month-by-month cycle

| Month | Kya hota hai | Kyun |
|-------|--------------|------|
| **Month 1** | **Site 1 → Site 2** — Site 2 launch (no links yet) | Site 2 ko ranking + crawl me time do |
| **Month 2** | **Site 1 → Site 2** (1 → 2) | Site 2 1 month old ho chuka hai → link safe hai |
| **Month 3** | **Site 2 → Site 1** (2 → 1) | **Link back** — Site 1 ko Site 2 se bhi support |
| **Month 4+** | **Site 3 → Site 1** | Same cycle repeat (Site 3 → Site 1 → ... ) |

### Flow diagram

```
month 1        month 2        month 3         month 4+
┌───────┐     ┌───────┐      ┌───────┐       ┌───────┐
│ Site 1│     │ Site 1│      │ Site 2│       │ Site 3│
│  →    │     │  →    │      │  ←    │       │  →    │
│ Site 2│     │ Site 2│      │ Site 1│       │ Site 1│
└───────┘     └───────┘      └───────┘       └───────┘
Launch Site 2  1 → 2           2 → 1            Repeat
no links yet  Site 2 is       link back        same cycle
              1 month old
```

### Rules / Notes

- ✅ **Only 1 link edit per month** — ek site me ek mahine me sirf ek link change
- ✅ Link **sirf 1 month purane** site ko do (naya site abhi link mat lo)
- ✅ Link **content ke andar** jaye, footer me nahi
- ✅ Anchor text **natural** ho — "here", "click this" mat, keyword use karo
- ❌ Ek hi page pe 10 links mat daalo
- ❌ Same anchor text se same target pe baar-baar link mat karo
- ❌ Sab sites ko ek hi din link mat karo
- ❌ Paid link exchange / PBN mat karo

### AI prompt (interlink add karne ke liye — 1 site, 1 link)

```text
Add one contextual backlink from this website to my other tool. In the content where
it fits naturally, link the phrase "[target keyword]" to "[target URL]" using a plain
<a> tag with descriptive anchor text. Add it in only ONE place — inside the body copy,
not in the footer or a link list. Keep the sentence natural and helpful, and do not
add "rel=nofollow". Then tell me exactly where you added it and why that spot fits.
```

---

## 4. Track your backlinks 📊

| Tool | Link | Kya dikhta hai |
|------|------|----------------|
| **Ahrefs Backlink Checker** | https://ahrefs.com/backlink-checker | Free — apne domain ka backlink profile + top linking pages |
| **Google Search Console → Links** | https://search.google.com/search-console/links | Google ki hi truth — top linking sites + anchor text |
| **Bing Webmaster → Links** | https://www.bing.com/webmasters/help/links-report-12184f8b | Bing ka link report |
| **Ahrefs blog — backlinks** | https://ahrefs.com/blog/backlinks/ | Types + strategy deep dive |
| **Ahrefs blog — link building** | https://ahrefs.com/blog/link-building/ | Practical playbook |

### Kaise check karein (har month)

1. https://ahrefs.com/backlink-checker → apna domain daalo
2. Dekho: **Referring domains**, **Backlinks**, **Do-follow %**
3. Compare with pichle month — **dono hi badhne chahiye**
4. Search Console → Links → **Top linking sites** — yahan Google ka data hai (sabse accurate)
5. Naye spam links dikhein toh **disavow** consider karo (Google disavow tool: https://support.google.com/webmasters/answer/2648487)
6. Apne interlinks bhi verify karo — sahi site se sahi page pe ja raha hai ya nahi

### Spreading sheet (har site ke liye)

| Site | Month | Link added to | Target | Anchor text | Type | Live? |
|------|-------|---------------|--------|-------------|------|-------|
| Site 1 | 2 | `/tool-a` | Site 2 `/tool-b` | "online ruler" | Editorial | ✅ |
| Site 2 | 3 | `/tool-b` | Site 1 `/tool-a` | "unit converter" | Editorial | ✅ |
| Site 3 | 4 | `/tool-c` | Site 1 `/tool-a` | "pdf tool" | Editorial | ✅ |

---

## 5. Extra tips

- 🔗 **Anchor text variety** — same anchor mat do ek hi target pe (brand + keyword + URL mix)
- 📄 **Content hi link hai** — 600-word tool page + FAQ + guide = link-worthy content
- 🌍 **Referral traffic** track karo — GA4 me "Referral" source se aata hai
- ⚡ **Faster indexing** — IndexNow + internal links = bots jaldi crawl karte hain
- 🧠 **Helpful content** likho, spam mat karo: https://developers.google.com/search/docs/fundamentals/creating-helpful-content

---

## ✅ Backlink Checklist

- [ ] Har site me **"Related tools"** section live hai (2-3 internal links, content ke andar)
- [ ] Anchor text **keyword match** kar raha hai, generic "click here" nahi
- [ ] **Interlink Slowly** follow ho raha hai — 1 link edit per month, 1 month purane site ko
- [ ] Month 3 pe **link back** diya (Site 2 → Site 1)
- [ ] Naye site ko link **turant nahi** diya — 1 month wait kiya
- [ ] Ahrefs Backlink Checker se profile check kiya
- [ ] Search Console → Links me top linking sites dekhe
- [ ] Spreading sheet bana hai (har link track ho)
- [ ] Paid links / PBN / mass directory submit **nahi** kiya
- [ ] Anchor text mix: brand + keyword + URL — sab same nahi

---

## 🔗 Saare Links (quick reference)

| Kaam | Link |
|------|------|
| **Track backlinks (Ahrefs)** | https://ahrefs.com/backlink-checker |
| Search Console — Links report | https://search.google.com/search-console/links |
| Bing — Links report | https://www.bing.com/webmasters/help/links-report-12184f8b |
| Google — link spam policy | https://developers.google.com/search/docs/essentials/spam-policies#link-spam |
| Google — helpful content guide | https://developers.google.com/search/docs/fundamentals/creating-helpful-content |
| Ahrefs — backlinks guide | https://ahrefs.com/blog/backlinks/ |
| Ahrefs — link building playbook | https://ahrefs.com/blog/link-building/ |
| Google disavow tool | https://support.google.com/webmasters/answer/2648487 |
| How Google Search works | https://www.google.com/search/howsearchworks/ |
| IndexNow (faster indexing) | https://www.indexnow.org/documentation |

---

## Deliverable

A **self-referencing backlink network** — har site dusri site ko link karti hai (1 per month,
slowly), tracked with Ahrefs + Search Console, aur zero paid links. Higher rankings,
referral traffic, faster indexing, aur long-term brand authority. 🔗
