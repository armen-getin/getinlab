# Deploying getinlab.ai + the private deck link

The site (`website/index.html`) and the deck (`decks/getin-deck-seed.html`) are both **self-contained HTML**
(fonts load over HTTPS; the deck inlines everything; the site's only asset is `assets/murmuration.webp`).
Two facts shape the options below:

- **Wix has no code-size limit on HTML embeds**, but **Wix will not let you upload a raw `index.html` to replace a page** — embeds run inside an `<iframe>` within the Wix page. ([Wix: embedding HTML](https://support.wix.com/en/article/wix-editor-embedding-a-site-or-a-widget), [static vs dynamic](https://support.wix.com/en/article/static-vs-dynamic-sites))
- A real static host (**Cloudflare Pages** — free, unlimited bandwidth, custom domain + free SSL, drag-and-drop `index.html`) hosts the file as an actual full page. ([Cloudflare Pages](https://www.craftedtemplate.com/blog/how-to-deploy-a-site-on-cloudflare-pages))

---

## Deploy the website

### Option A — Fastest: replace the existing Wix embed
Use this if your current getinlab.ai is the single-file site pasted into a Wix **Embed HTML / HTML-iframe** element.

1. Wix Editor → select the HTML/Embed element showing the current site → **Settings → Enter code**.
2. Delete the old code and paste the contents of **`2026-06-17-getin-website-standalone.html`** (the single-file build — image inlined, nothing else to upload).
3. Stretch the element to full width; set the iframe `width`/`height` to `100%`. **Publish.**

Trade-off: it renders **inside** Wix's page chrome and inside an iframe, so it isn't truly full-bleed, and mobile sizing in iframes sometimes needs tweaking. For a clean full-page result, use Option B.

### Option B — Recommended: Cloudflare Pages (free, full-page)
1. Create a free Cloudflare account → **Workers & Pages → Create → Pages → Upload assets (Direct Upload)**.
2. Drag the **`website/` folder** (`index.html` + `assets/murmuration.webp`). Deploy → you get a `*.pages.dev` preview URL.
3. **Custom domains** → add `getinlab.ai` and `www.getinlab.ai`; Cloudflare walks you through the DNS change. (If the domain is registered through Wix, you'll point its nameservers/records at Cloudflare — i.e. move hosting off Wix.)
4. Publish. Full-bleed, global CDN, free HTTPS.

Trade-off: this moves getinlab.ai's hosting off Wix. If you want to keep the rest of the site on Wix, keep Wix and only host the **deck** on Cloudflare (see below).

> **Before going public:** the site currently has `<meta name="robots" content="noindex, nofollow">` in `<head>`. Leave it while testing; **remove that line when you want Google to find getinlab.ai.** (Keep the deck's noindex — see below.)

---

## The pitch deck at a private / unlisted link

> **Honest caveat.** An unguessable URL is **security by obscurity** — it keeps the deck out of search and casual
> discovery, but **anyone who has the link can open or forward it**, and links can leak via browser history or
> referrer headers. It is *not* access control. For real control, use a password or a per-viewer share tool (below).

The deck is already hardened against indexing (`noindex, nofollow, noarchive, nosnippet`) and is self-contained.

**Suggested unguessable path:** `getinlab.ai/d/f1bb19ebcb08371c96afa06f`
(regenerate any time — it's just a random string; the point is that it's not guessable and not linked anywhere.)

### On a static host (Cloudflare Pages / Netlify)
1. Place `getin-deck-seed.html` at `/d/f1bb19ebcb08371c96afa06f/index.html` (or name the file `<slug>.html`).
2. Add a `robots.txt` at the site root: `User-agent: *` then `Disallow: /d/`.
3. Don't link it from any page.
4. **Stronger:** turn on **Cloudflare Access** (free, up to 50 users, email one-time-code) on the `/d/*` path — now only emails you approve can open it, and you can revoke anytime.

### On Wix
1. Add a new page; under **Page → SEO**, turn **off** "Let search engines index this page" (adds noindex). ([Wix: prevent indexing](https://support.wix.com/en/article/wix-editor-preventing-search-engines-from-indexing-your-page))
2. Give the page a random URL slug; **hide it from the menu**.
3. **Strongest on Wix:** **Page → Permissions → Password Protect** the page — password-protected pages are not crawled or indexed and need the password to open. ([Wix: password protect](https://wiksit.com/blog/how-to-password-protect-a-wix-page))
4. Put the deck on the page via an Embed HTML element, or link out to the hosted file.

### The most common founder setup (worth considering)
For investor decks, many founders use a **deck-sharing tool** — DocSend, Pitch, or Brieflz — which gives an
unguessable private link **plus** per-viewer analytics (who opened it, how long on each slide) and one-click
revoke. That's the robust version of what you're describing. You'd export the deck to PDF (open it in a browser
→ Print → Save as PDF) and upload there.

### "A reference on the website"
- Want a discreet door? Add a low-key footer link (e.g. **Investors**) pointing to the **password/Access-protected**
  deck — visible but gated.
- Want it fully invisible? Don't reference it on the site at all; just share the raw unguessable link directly.

---

## Files prepared for you
- **`2026-06-17-getin-website-standalone.html`** — the entire site as one self-contained file (image inlined).
  For the Wix-embed path or any drag-and-drop host.
- **`website/index.html` + `website/assets/`** — source for static-host deploys (Cloudflare/Netlify/GitHub Pages).
- **`decks/getin-deck-seed.html`** — already self-contained + `noindex`; host at the unguessable path above.
