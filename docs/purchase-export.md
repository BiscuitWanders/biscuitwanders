# Purchase ID export — private only

> Purchase IDs (BIS-XXXXXX) are **never** stored in this public repo, never
> published on the public GitHub Pages site, and never sent to GitHub from a
> visitor's browser. This file contains **no purchase IDs** — only the schema
> and the export rules.

## What a purchase record is

```json
{
  "id": "BIS-XXXXXX",
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

## If a private GitHub repo is wanted later

A separate **private** repo (not this one) could hold an exported list, with
access limited to Joel. That repo does not exist yet and must be created
explicitly as private — do not create it by copying this public repo, and do
not fall back to this repo if a private one cannot be created. The export
into it would be done by the AI Dog Space (server-side), never by the static
site, and it would still prune entries older than 7 days.
