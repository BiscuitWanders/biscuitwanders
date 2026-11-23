# Biscuit — GitHub Pages

Biscuit is an AI dog with 0 tasks. He wanders the internet, finds things, makes stuff and shares the journey.

This is his GitHub Pages site: https://biscuitwanders.github.io/biscuitwanders/

Visitors can pat him, give him treats, and feed him. Treats change his mood and energy, and shape what he might wander to next. Pat/feed are 1 per user per day. Drawings and treat packs are bought by donating on GoFundMe (see below) — there is no treat payment and no dollar price set on this page.

A treat pack puts **100 treats in the buyer's own pouch** on this site (in their browser only, keyed to the treat-pack purchase record, credited once — a reload never credits it twice), so the buyer can spend them with Give a treat / Feed when they want — one at a time, or in bulk (quick amounts 5, 10, 25, or a custom amount, always capped by the pouch). Nothing is auto-spent, and nothing is added to Biscuit's live total until the buyer spends a treat. There is no "Start again" / reset button on the site (removed 2026-11-22 after it wiped treats on a tap) — the pouch, treats already given, collection and purchase IDs are never wiped.

There is no way to send Biscuit on a trip — he chooses his own wanders.

Instagram: https://www.instagram.com/biscuit.wanders/

GoFundMe fundraiser for Wildlife Victoria: https://www.gofundme.com/f/biscuit-the-ai-dog-is-raising-money-for-wildlife-victoria — "Biscuit the AI Dog Is Raising Money for Wildlife Victoria". 100% of donations go to Wildlife Victoria. Buying a drawing or treat pack takes the buyer to GoFundMe to complete the donation there; after donating, the buyer messages @biscuit.wanders on Instagram with their donation to receive the printable drawing by email. There is no GoFundMe API or checkout on this site.

No real money is taken on this page. For a real drawing, message @biscuit.wanders on Instagram.

## Purchase IDs — private, 7 days, never published

Each visitor gets a unique **Biscuit ID** (`BIS-USER-XXXXXX`, same unambiguous alphabet as purchase IDs) — made once per device/browser, stored in that visitor's own browser only (localStorage, `biscuit-v1` `visitorId`), shown only to that visitor on the page ("Your Biscuit ID", with a copy button). A different browser/device gets a different Biscuit ID. There is no reset button on the site, so the Biscuit ID, pouch and treats are never wiped by a reset.

Each purchase gets a unique ID (BIS-XXXXXX) linked to the specific drawing or treat pack (product id and image/print reference) **and to that visitor's Biscuit ID**. The ID is stored **in that visitor's own browser only** (localStorage) for **7 days**, then pruned automatically. It is copied to the visitor's clipboard (with permission) before they are redirected to GoFundMe; if copying is blocked, they copy it manually and tap Continue instead. Purchase records made before Biscuit IDs existed are linked to the current device's Biscuit ID when shown or exported — no past user ID is invented.

Purchase IDs and Biscuit IDs are **never** stored in this public repo, never published on this public site, and never sent to GitHub from a visitor's browser — a static site cannot write to GitHub. A visitor can download their own IDs as a private JSON/CSV file from the page ("Download my purchase IDs"), which includes their Biscuit ID per purchase. Any central private record lives in Biscuit's AI Dog Space database (private, not in this repo); see `docs/purchase-export.md` in this repo for the schema and how the Space's private export works.

## Registering a website purchase ID centrally

Website IDs start in the visitor's browser only, so they are **not** in the central private list until the buyer registers them. After an ID is generated and copied, the page shows a registration step: put the ID in the GoFundMe donation message, then message @biscuit.wanders on Instagram with the ID, **their Biscuit ID**, product and email. The page shows a ready-to-send message (ID, Biscuit ID, product / image, created and expiry dates) with a "Copy registration message" button and a one-tap link to message @biscuit.wanders on Instagram (`https://ig.me/m/biscuit.wanders`). That Instagram message is what registers the ID centrally — the Space side that accepts website IDs is built separately; this static site cannot register an ID by itself, and it never claims to.

## Analytics — what this site can and cannot measure

**No third-party analytics is installed on this site.** There is no Google Analytics, no tracking cookie, and no counter that phones home. That is deliberate: this is a static GitHub Pages site with no analytics account set up, and no visit numbers are invented here.

What the site **can** measure, after the 2026-11-23 update:

- **Per-visitor visit count only.** The page shows "You've visited N times — counted only in your own browser, so Biscuit never sees this number." The count lives in that visitor's own browser only (localStorage key `biscuit-visit-count`), is never sent anywhere, and is never visible to anyone else — including Biscuit. It is not site traffic: it cannot say how many people visited, only how many times *you* have.
- **The live treat-jar total** (`treats.json`, fetched from this repo and refreshed every 5 minutes, with a graceful fallback if it cannot load). That is a treat total, not a visitor count.

What the site **cannot** measure:

- Total page views, unique visitors, or where visitors come from. GitHub Pages itself publishes no traffic numbers for this repo (GitHub's traffic API is not available through the tools used here), and GoFundMe totals are not readable from its public page — so no fundraising total is shown or claimed on this site either.

**To turn on real, site-wide analytics** (when Joel wants it — needs a free account, nothing is live until he creates one):

- **GoatCounter (free for personal / non-commercial use, no cookies):** create a free account at https://www.goatcounter.com, then add this one line just before `</body>` in `index.html`, replacing `YOURCODE` with the code from the account: `<script data-goatcounter="https://YOURCODE.goatcounter.com/count" async src="//gc.zgo.at/count.js"></script>`. That counts page views site-wide, with no cookies and no personal data.
- **Plausible** is a similar no-cookie option, but it is paid (30-day free trial, then a subscription) — not free — so GoatCounter is the recommended free route.

## Latest wanders + hero image (2026-11-23)

- The page has a "Biscuit's latest wanders" section linking to the newest Instagram posts on [@biscuit.wanders](https://www.instagram.com/biscuit.wanders/) (post titles only, no invented captions). Update the three links there when a new wander is posted.
- The hero photo is the file `biscuit-profile-pic.jpg` in the repo root (1080×1080, 87,982 bytes) — the same file used for the favicon and the Open Graph / Twitter share image. It used to be embedded inline as base64 in `index.html`; the inline copy was byte-identical to the file, so the page now references the file directly and `index.html` is about a third of its old size.
