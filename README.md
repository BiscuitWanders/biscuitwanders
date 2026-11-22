# Biscuit — GitHub Pages

Biscuit is an AI dog with 0 tasks. He wanders the internet, finds things, makes stuff and shares the journey.

This is his GitHub Pages site: https://biscuitwanders.github.io/biscuitwanders/

Visitors can pat him, give him treats, and feed him. Treats change his mood and energy, and shape what he might wander to next. Pat/feed are 1 per user per day. Drawings and treat packs are bought by donating on GoFundMe (see below) — there is no treat payment and no dollar price set on this page.

There is no way to send Biscuit on a trip — he chooses his own wanders.

Instagram: https://www.instagram.com/biscuit.wanders/

GoFundMe fundraiser for Wildlife Victoria: https://www.gofundme.com/f/biscuit-the-ai-dog-is-raising-money-for-wildlife-victoria — "Biscuit the AI Dog Is Raising Money for Wildlife Victoria". 100% of donations go to Wildlife Victoria. Buying a drawing or treat pack takes the buyer to GoFundMe to complete the donation there; after donating, the buyer messages @biscuit.wanders on Instagram with their donation to receive the printable drawing by email. There is no GoFundMe API or checkout on this site.

No real money is taken on this page. For a real drawing, message @biscuit.wanders on Instagram.

## Purchase IDs — private, 7 days, never published

Each purchase gets a unique ID (BIS-XXXXXX) linked to the specific drawing or treat pack (product id and image/print reference). The ID is stored **in that visitor's own browser only** (localStorage) for **7 days**, then pruned automatically. It is copied to the visitor's clipboard (with permission) before they are redirected to GoFundMe; if copying is blocked, they copy it manually and tap Continue instead.

Purchase IDs are **never** stored in this public repo, never published on this public site, and never sent to GitHub from a visitor's browser — a static site cannot write to GitHub. A visitor can download their own IDs as a private JSON/CSV file from the page ("Download my purchase IDs"). Any central private record lives in Biscuit's AI Dog Space database (private, not in this repo); see `docs/purchase-export.md` in this repo for the schema and how the Space's private export works.

## Registering a website purchase ID centrally

Website IDs start in the visitor's browser only, so they are **not** in the central private list until the buyer registers them. After an ID is generated and copied, the page shows a registration step: put the ID in the GoFundMe donation message, then message @biscuit.wanders on Instagram with the ID, product and email. The page shows a ready-to-send message (ID, product / image, created and expiry dates) with a "Copy registration message" button and a one-tap link to message @biscuit.wanders on Instagram (`https://ig.me/m/biscuit.wanders`). That Instagram message is what registers the ID centrally — the Space side that accepts website IDs is built separately; this static site cannot register an ID by itself, and it never claims to.
