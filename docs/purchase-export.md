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
  "expiresAt": "2026-11-29T07:00:00.000Z"
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
