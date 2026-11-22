# Purchase ID export — private only

> Purchase IDs (BIS-XXXXXX) are **never** stored in this public repo, never
> published on the public GitHub Pages site, and never sent to GitHub from a
> visitor's browser. This file contains **no purchase IDs** — only the schema
> and the export rules.

## What a purchase record is

```json
{
  "id": "BIS-XXXXXX",
  "visitorId": "BIS-USER-XXXXXX",
  "item": "Flying Scotsman",
  "kind": "drawing",
  "productId": "scotsman",
  "imageRef": "PRINTS.scotsman / scotsman-printout.svg",
  "createdAt": "2026-11-22T07:00:00.000Z",
  "expiresAt": "2026-11-29T07:00:00.000Z",
  "pouchCredited": false,
  "pouchTreats": 100
}
```

- `id` — unique purchase ID, shown to the buyer and put in their GoFundMe
  donation message.
- `visitorId` — the buyer's unique Biscuit ID (`BIS-USER-XXXXXX`, made once
  per device/browser, stored in that visitor's own browser only, shown only
  to that visitor). Every purchase ID is linked to the visitor's Biscuit ID.
  Purchase records made before Biscuit IDs existed are linked to the current
  device's Biscuit ID when shown or exported — no past user ID is invented.
- `item` / `kind` — the drawing or treat pack name and type.
- `productId` / `imageRef` — the specific image/product the ID belongs to, so
  an ID shown or matched later is always clear about which drawing it is for.
- `createdAt` / `expiresAt` — `expiresAt` is `createdAt` + **7 days**.

## Treat packs — 100 treats in the buyer's pouch

A treat-pack purchase puts **100 treats in that visitor's own pouch** on
this site (localStorage, in their browser only), to spend with Give a treat
/ Feed when they want. The credit is keyed only to the treat-pack purchase
record in that browser and is marked `pouchCredited` on the record, so a
reload never credits it twice; a browser with no such record gets nothing.
Nothing is auto-spent and nothing is added to the live total (`treats.json`)
until the buyer spends a treat. The pouch total is shown on the page
("Your treat pouch, X treats") and the purchase list / purchase ID box
show that the pack's treats are in the pouch, ready to use. The private
JSON/CSV export includes `pouchCredited` / `pouchTreats` per purchase.
No purchase ID or user ID is named in the site code for this — the credit
follows the purchase record. Note the honest limit: this static site
cannot verify a GoFundMe donation (no API), so the pouch credit follows
the purchase record made in the browser, same trust model as the rest of
the site. "Start again" keeps the pouch if it holds treats credited
from a treat pack.

## Bulk treating

Give a treat also works in bulk: quick amounts 5, 10 and 25, plus a custom
amount input. Every bulk give is capped by the visitor's own pouch — the
amount must be a whole number, at least 1, and no more than the pouch holds;
an invalid amount is refused with an on-page message and nothing changes.
A successful bulk give moves that many treats from the pouch into Biscuit's
fund/energy in one go (same per-treat effect as a single give) and persists
in localStorage. Pat/feed daily limits (1 per user per day) still apply to
Pat and Feed only, not to bulk treating. Nothing is added to the live total
(`treats.json`) until the visitor actually spends a treat.

## Retention

- **7 days.** Entries older than 7 days are pruned automatically:
  - on the site, in the visitor's own browser (localStorage, key `biscuit-v1`)
    on load and whenever a purchase is started;
  - in any private central list, by the job that maintains it (see below).
- "Start again" on the site keeps only unexpired (under 7 days) IDs.

## Where IDs actually live

1. **Per visitor, in their own browser only** (localStorage). This is the
   only place the static Pages site can store them — a static site cannot
   write to GitHub from a visitor's browser.
2. **Private export, per visitor only.** On the site, "Download my purchase
   IDs (JSON/CSV)" downloads that visitor's own IDs to their own device.
   Nothing is uploaded or published by that download.
3. **Central private record — AI Dog Space only.** The AI Dog Space
   (`ai-dog`) has server-side storage: its `startGoFundMePurchase` action
   stores each purchase in its private `gofundme_purchases` table (unique
   purchase code, listing, treat price, 100% to Wildlife Victoria). That is
   the only central list, and it is **not** in this public repo and is not
   published. Joel can see it in the Space's stall view.
   - The Space's list must also prune entries older than 7 days (a
     Space-side job, not a file in this repo). Do not copy that list into
     this repo.

## What NOT to do

- Do not commit `purchases.json`, `purchases.csv`, or any file containing
  real purchase IDs to this public repo.
- Do not add a GitHub Action / workflow here that writes purchase IDs into
  this repo — that would publish them.
- Do not add purchase IDs to the public site's HTML.

## The private GitHub list — and how website IDs get into it

A separate **private** repo now exists, created by Joel:
`BiscuitWanders/Biscuitwanders-purchases` (verified private, empty at
creation). The AI Dog Space pushes its private 7-day export snapshot
(`purchases.json` + `purchases.csv`) into that repo on a schedule, pruning
entries older than 7 days. Nothing is ever pushed to this public repo.

**The gap, and the registration route:** IDs generated on this static site
start in the visitor's own browser only — the site cannot write to GitHub or
to the Space from a visitor's browser, so a website ID is **not** in that
private repo until the buyer registers it. Registration is the buyer's
Instagram message: after an ID is generated and copied, the page shows a
registration step with a ready-to-send message (ID, the buyer's Biscuit ID,
product / image, created and expiry dates) plus a one-tap link to message
@biscuit.wanders on Instagram. That message is what gets the ID into the
central private list — the Space side that accepts website IDs is built
separately, and this site does not claim to register an ID by itself.

## Live treat total — public aggregate only (`treats.json`)

The page now shows Biscuit's live treat jar total, fetched from the public
`treats.json` published alongside this site
(`https://biscuitwanders.github.io/biscuitwanders/treats.json`).
It contains ONLY aggregate numbers — `totalTreats`, `goalTreats`,
`updatedAt` — and no purchase IDs, user IDs, names or emails. The page
shows the live total ("Biscuit's treat jar: X treats" / "Wander fund —
firefly meadow: X / 80 treats" + last updated) separately from the
visitor's own local counts on this device, which are unchanged. If the
file is missing or the fetch fails, the page shows "Live total not
available yet" and keeps working on local counts. The Space publishes
`treats.json` from Biscuit's real treat jar; this static site cannot
update it itself — the file in this repo is the published snapshot.
