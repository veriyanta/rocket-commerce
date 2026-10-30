---
name: "rocket-audit"
description: "Run a ROCKET Social Commerce OS audit for a client from an evidence pack: pre-score all 80 items, fill a copy of the audit workbook, and draft the readout and 90-day plan."
---

# ROCKET audit

This skill runs a full ROCKET audit for one client brand in one market. Claude does the evidence handling, calculations and first-pass scoring. The auditor (Veriyanta or a licensed auditor) confirms every score before anything goes to the client. Never present a Claude pre-score as a final score.

## What you need before starting

- Client name, market, platforms in scope, audit period, audit owner on the client side.
- The evidence pack: a Google Drive folder, a folder on the connected computer, or files uploaded to the chat.
- The workbook template: Google Sheet `1nQ6Blz26ZjDi0KItjbiB6-8zsZMSpAGkxW05gt5UUAQ` ("ROCKET – Audit & Operations Workbook"). Never write client data into the template itself.

If any of the first three are missing, ask once, in one message.

## Steps

1. **Copy the workbook.** Drive `copy_file` the template to `ROCKET Audit – {Client} – {Market} – {Period}`. Fill `Setup!B4:B10` (brand, market, platforms, period, date, owner, reviewer). Keep default weights unless the client is clearly at Starter stage, then propose raising L and RO and ask before changing.
2. **No evidence pack yet?** Generate the evidence request (section "Evidence request" below), tailored to the platforms in scope, as a doc the client can work through. Stop there.
3. **Inventory the pack.** List every file. Map each file to the item IDs it supports using the item map. Note which items have no evidence.
4. **Score the Auto items in code.** Load exports with pandas (run Python with `-I`, files in their own folder). Apply the rule in the item map exactly. Never estimate a number by reading it. Keep the calculation (inputs, result, threshold) for the notes column.
5. **Run the creative review** for C2 and RO4 (section "Creative review" below). It fills the `Creative Review` tab, which gives the suggested scores for those two items.
6. **Build the finance views** for L8, L14, L15 and T5 (section "Finance and unit economics" below). They fill the `Unit Economics` and `Store P&L` tabs.
7. **Pre-score the Assisted items.** Read the document or screenshot, apply the rubric, and write a one-line rationale that cites the file (and page or sheet). Give a confidence: High, Medium or Low.
8. **Manual items.** Leave the score blank and add the interview questions to the notes column.
9. **Write to the workbook.** In `Audit Checklist`, rows 2 to 81 hold L1 to T13 in the item map's order (L1-L17 are rows 2-18, RO4 is row 22, C2 is row 32, T1 is row 69, T5 is row 73). Check the ID in column A before writing. Write column F (score 0-3), G (evidence: file name and location), J (notes: `Pre-score by Claude. <rationale or calculation>. Confidence: <level>.`). Leave G empty where there is no evidence; the sheet then caps the item at 1. Do not touch columns K and L (formulas).
10. **Read the scorecard back.** `get_values` on `Scorecard!A4:J17`. Use these numbers in everything you report; never recompute them yourself.
11. **Fill the Action Log.** One row per failed critical control (CC) and per item scored 0 or 1, with columns A (action ID A-001...), B (item ID), E (gap found), F (proposed action), G (proposed owner), I (`Not started`). Columns C and D fill themselves. Delete the example row A-001 first.
12. **Draft the readout.** ROCKET index and stage, pillar status, failed critical controls, top five gaps (failed CCs first, then lowest scores in the heaviest-weighted pillars), the Creative and PDP Quality Scores with the three weakest rubric criteria for each, the store profit picture (profit after ads and after fixed costs, SKUs below breakeven, the touchpoint split with its caveat), and a 90-day plan split into days 1-30 (fix CCs and Launchpad, including finance), 31-60 (fix RO and E), 61-90 (scale C, K, T). Make it a doc unless the user asks for slides.
13. **Hand over for review.** List the items the auditor must confirm: CCs first, then Low confidence, then Medium. State plainly how many items are pre-scored, auto-scored, confirmed and still blank.

## Scoring rubric (0-3)

- **0 Missing:** not done, or no proof at all.
- **1 Basic:** done, but ad hoc or person-dependent; or any item without evidence.
- **2 Managed:** written standard, named owner and target, done every cycle.
- **3 Leading:** tested or benchmarked, improving quarter on quarter, partly automated.

A critical control passes at 2 or more. For Auto items the rule column gives the score directly.

## Creative review (C2 and RO4)

Creative is measured twice: what an asset contains (a rubric scored 0-2 per criterion) and what it does (performance). The `Creative Review` tab holds both and computes the suggested audit score.

**Videos (rows 15-34).** Take 20 recent videos: about half from the brand account and half from affiliates, mixing top and bottom performers by GMV. For each, view key frames (first 3 seconds, middle, end) and read the transcript or captions. Score columns F-L 0-2:

- F Hook (0-3s): product or problem visible in the first 2 seconds, plus a text or spoken hook and movement.
- G Native: 9:16, 720p or higher, person on camera, platform-native style, sound used.
- H Proof: demo, before and after, texture, comparison or test shown.
- I Brand: brand or product named early, ideally spoken.
- J Offer and CTA: price or voucher shown, clear instruction, correct product tagged.
- K Search: target query spoken, shown as text and in the caption.
- L Compliance: claims within the claims sheet and disclosure label on paid posts. A 0 fails the video.

Also fill A (asset ID), B (link), C (SKU), D (format such as demo, review, unboxing, before-after), E (creator tier), and from analytics O (6-second view rate), P (product-click rate), Q (GPM). Put a one-line reason for any 0 in R.

**Product pages (rows 39-58).** Take the top 20 pages by GMV. Use mobile screenshots of the full page. Score columns F-M 0-2:

- F Main image: recognisable at thumbnail size, clean background, no clutter.
- G Image sequence: images answer questions in buying order (what it is, benefit, how it works, proof, size and box contents, comparison, trust).
- H Benefits: top three benefits visible without scrolling; text in images legible on a phone and in the local language.
- I Video: short demo video on the page.
- J Title: buyer's search term first, 40-150 characters, no keyword stuffing (check length in code).
- K Trust: registration numbers where required, official badge, warranty, rating.
- L Message match: repeats the main claim and visual of the creator videos that drive traffic to it.
- M Variants: sizes, colours and bundles easy to tell apart.

Also fill A-E (SKU, link, platform, role, product name) and from analytics P (card click-through), Q (page conversion), R (pre-sale chats per 100 orders).

**Rules.** Delete the example rows (V-001, SKU-001) first. Leave the weights in rows 13 and 37 at default unless the brand has a calibrated set from a previous audit. Read `'Creative Review'!A4:C10` back: B10 and C10 are the suggested scores for C2 and RO4; write those into the checklist with the tab as evidence. If the correlation cells show a value, report it; a weak or negative correlation means the weights need recalibrating for this brand, which goes in the readout as a recommendation.

**Calibration (quarterly, retainer clients).** Score 40-60 assets, compare the criteria averages of the top and bottom halves by GPM (videos) or page conversion (pages), propose new weights, and write the patterns that separate winners into a short "what works" brief for creators.

## Finance and unit economics (L8, L14-L17, T1, T5)

The store figure decides; the channel and touchpoint splits only point direction, because levers feed each other (LIVE needs ads and offers; GMV Max boosts brand, affiliate and product-card traffic at once).

**Unit Economics tab (L14, T1, T5).** Delete the example row first. One row per promoted SKU and channel, from row 8. Inputs: list price, seller discount, landed cost, platform commission, dynamic commission and its cap, programme fees, fixed fees per order (processing + logistics), fulfilment, return and failed-delivery provision, affiliate share of orders, affiliate commission, samples per order, and the current GMV Max ROI target converted to a net-revenue basis. Take fee rates from the client's own settlement reports, not from published rate cards. Set `B4` (target margin after ads) and `B5` (planned ad share) with the client. Read back columns V (breakeven ROI), W (target ROI), X (max commission on a boosted affiliate order), Y (price floor), AA (ROI check) and AB (status).

**Store P&L tab (L8, L15).** Delete the example values first (C6:I25 and C32:I51, plus B3). Enter last month's figures in one unit. Touchpoint view: revenue and layer 1 costs from Seller Center orders by content type and source; affiliate costs to affiliate columns; GMV Max by the platform's own split (LIVE GMV Max to LIVE, Product GMV Max by creative source), else by GMV share; LIVE ops to Brand LIVE; content production to Brand video; KOL fixed fees to affiliate; agency, enabler, tools and team to Unallocated. Channel view: same lines by platform. Read back `B57:B58`: both must say OK before you report anything.

**Do not** recommend cutting a touchpoint on its allocated profit alone; recommend a test (T4) instead.

## Platform thresholds

Record the thresholds shown on the client's own Account Health pages in `Setup` notes at each audit; they differ by market and change often. Reference values for TikTok Shop: late dispatch rate (LDR) at or below 4%, seller-fault cancellation rate (SFCR) at or below 2.5%, valid tracking rate (VTR) at least 95%. For Shopee and Lazada, use the thresholds shown in their seller centers (non-fulfilment rate, late shipment rate, chat response rate).

## Item map

Mode: **A** = Auto (calculated from exports), **S** = Assisted (Claude pre-scores from documents, screenshots or frames), **M** = Manual (interview).

| ID | Mode | Evidence | Rule |
|---|---|---|---|
| L1 | S | RACI, SOWs | 3: all 11 functions one R + reviewed this quarter; 2: all 11 one R; 1: partial; 0: none |
| L2 | S | Business Center users and assets | 2+: brand entity is Admin on BC, shop, official account, ad accounts, catalog; 0-1 if any held only by a partner |
| L3 | S | Access register vs user lists | 2+: every user named, no generic or shared emails; 0 if any shared login |
| L4 | A | Ads Manager campaign list | 2: one primary ad account and no Video or Product Shopping Ads on GMV Max products; 0 if any conflict |
| L5 | S | Authorization list | 2: every partner authorization has expiry and owner; 1: list exists, gaps |
| L6 | M | Access review log | Ask: last review date, leaver removal time |
| L7 | S | Last 4 weekly dashboards | 2: all 4 exist and cover GMV by touchpoint, shop health, ads, creators, stock; 1: partial |
| L8 (CC) | A | Store P&L, settlement reports | Compare the finance pack's net revenue and platform fees with settlement reports. 2: monthly P&L by profit layer, closed within 10 working days, gap 2% or less; 3: also split by channel and touchpoint; 1: GMV or revenue only; 0: none |
| L9 | M | Insight deck | Ask how insight feeds plans |
| L10 | S | Partner contracts | 2: every contract has exit and hand-back clause; 1: some |
| L11 | S | Compliance register | 2: covers commerce, data, claims rules for the market, reviewed in 90 days |
| L12 | S | Brand-protection sweep log; run your own search | Search the brand on each marketplace for unauthorised sellers and log what you find; 2: monthly sweep with takedowns closed |
| L13 | M | Decision RACI, crisis playbook | Ask who sets ROI targets, discount depth, creator fees |
| L14 | A | Unit economics sheet vs product list | Share of promoted SKUs with a current row (refreshed after the last fee, price or commission change). 2: 100%; 1: some; 0: none. Build the tab from the pack if the client has none |
| L15 | S | Finance pack, allocation key | 2: store P&L split by channel and touchpoint monthly with written keys and an unallocated line; 1: split without written keys |
| L16 | A | Returns, cancellation and 3PL reports, settlement report | 2: return, cancellation and failed-delivery cost tracked per SKU at actual, settlement timing and reserves known; record your computed rates |
| L17 | S | Business cases, post-reviews | 2: last campaign, LIVE programme, KOL deal and budget increase each had a pre-model and a post-review |
| RO1 | S | Assortment matrix vs product export | 2: at least 95% of active SKUs have a role, reviewed this quarter |
| RO2 | A | Product export (listing quality or fields) | Hero SKUs: title 40-150 chars, 5+ images, description 500+ chars, attributes complete. 2: all heroes pass; 3: all pass plus PDP video; 0: any hero at lowest tier |
| RO3 | S | PDP text vs claims sheet, registration numbers | Scan for banned claims; 2: none found and registrations shown where required; 0 if any banned claim |
| RO4 | S + A | Creative Review tab, top 20 pages | PDP Quality Score from the creative review: 3 if 80%+ of pages pass, 2 if 60%+, 1 if 30%+, else 0 (`'Creative Review'!C10`) |
| RO5 | S | Keyword map | 2: one per hero SKU, dated in last 90 days |
| RO6 | S | Analytics reports | 2: search and Shop Tab traffic and CVR reported separately every month |
| RO7 | A | Price export across channels vs corridor | Count prices below floor or above max discount in the last 30 days. 2: zero; 1: 1-3; 0: more |
| RO8 | S | Assortment plan | 2: exclusive packs defined for every major channel |
| RO9 | A | Inventory and sales export | Cover = stock / average daily units (last 28 days). 2: every hero 28+ days and no hero stock-out in 30 days; 3: also 56+ days before a campaign; 0: hero stock-out |
| RO10 | S | Dated storefront screenshot | 2: banners and showcase match current campaign |
| RO11 | M | Launch checklists | Ask for the last two launches |
| RO12 | S | Assortment matrix | 2: every SKU has a lifecycle stage and a dated decision |
| C1 | S | Content calendar | 2: next 4 weeks planned by pillar, format, SKU role |
| C2 | S | Creative Review tab, 20 videos | Creative Quality Score from the creative review: 3 if 80%+ of videos pass, 2 if 60%+, 1 if 30%+, else 0 (`'Creative Review'!B10`) |
| C3 | S | Test log | 2: hook variants tested for every hero concept |
| C4 | S | Claims sheet, captions or transcripts of 20 posts | 2: claims sheet exists and no banned claim found; 0 if any |
| C5 | S | 20 paid or affiliate posts | Check the commercial content label. 2: 100% labelled; 0 if any unlabelled |
| C6 | A | LIVE schedule vs LIVE analytics | Delivered sessions / planned. 3: 95%+; 2: 90%+; 1: 60-89%; 0: below 60% |
| C7 | S | Run-of-show files | 2: one per LIVE in sample with host, moderator, pinned sequence |
| C8 | S | Review notes with dates | 2: review within 24 hours for 90%+ of sessions |
| C9 | S | Weekly content report | 2: video GPM and product-click rate by asset weekly |
| C10 | A | Comment and DM export | 2: median response within agreed window, escalation SOP exists |
| C11 | S | Query map | 2: shared by content and listings, dated in last 30 days |
| C12 | S | Monthly report | 2: search-sourced GMV and query rankings reported |
| C13 | A | Run it yourself | Ask 10 category questions in web search and AI assistants; record brand mentions and errors. 2: log exists and fixes tracked; note your own result |
| K1 | S | Creator database | 2: tiers with written deal per tier |
| K2 | A | Affiliate Center export | Hero SKUs with both open and target plans and target rate above open. 2: 100%; 1: some; 0: none |
| K3 | A | Rates vs Unit Economics tab | 2: every rate at or below its SKU ceiling (column X for boosted posts) and ceilings dated this month |
| K4 | A | Affiliate settings, GMV Max product list | 2: Shop Ads commission set on every GMV Max product |
| K5 | A | Sample log | Sample-to-post rate. 2: tracked and 50%+; 1: tracked below 50%; 0: not tracked |
| K6 | S | Briefs | 2: every brief has approved and banned claims |
| K7 | S | Post sample, contracts | 2: all posts labelled and contracts require it; 0 if unlabelled paid posts |
| K8 | S | KOL contracts | 2: ad usage rights and take-down clauses in all |
| K9 | A | Affiliate export by creator | Compute top-10 share and month-on-month retention; 2 if these are already reported monthly; record your figures |
| K10 | A | GMV Max creative list | 2: affiliate posts present and refreshed in last 30 days |
| K11 | M | Program document | Ask about expert, employee and customer advocacy |
| K12 | S | Approval log | 2: expert content reviewed by regulatory or medical affairs |
| E1 | A | Account Health screenshots or export, 4 weeks | 2: score and all sub-metrics at or above threshold every week; 3: also above category benchmark; 0: any week below |
| E2 | A | Violation record | 2: no active points, past ones have root cause; 0: active points |
| E3 | A | Fulfilment dashboards | LDR and SFCR within threshold on every platform. 2: yes all 4 weeks; 0: any breach |
| E4 | A | Cancellation report with reasons | Oversell (out-of-stock) cancellations in last 30 days. 2: zero; 1: 1-5; 0: more |
| E5 | A | Chat performance | 2: response rate and time within platform target |
| E6 | S | QA log | 2: weekly scored samples |
| E7 | A | Reviews export | Share of negative reviews answered within 48 hours. 2: 90%+; flag SKUs below 4.6 stars |
| E8 | S | Returns report | Also analyse the export yourself: top 3 reasons. 2: monthly analysis with owners |
| E9 | S | Surge plan | 2: approved before last mega day |
| E10 | S | Spot-check log | 2: monthly checks logged |
| E11 | M | Journey map | Ask about post-purchase journey |
| E12 | S | VoC report; reviews and chat exports | Draft a VoC synthesis from the exports as part of the deliverable. 2: monthly report with closed actions |
| E13 | M | SOP, chat report | Ask about chat or WhatsApp handoff |
| T1 | A | Ads change history, Unit Economics tab | 2: ROI targets traceable to column W of the Unit Economics tab and no change held under 72 hours in the last 90 days; 1: one of the two; 0: neither |
| T2 | A | Campaign report | 2: profitable campaigns below 80% budget consumption, or documented reason |
| T3 | A | Creative library with dates | 2: new creatives added every week of the last 4 |
| T4 | S | Test readouts | 2: at least one incrementality test this quarter |
| T5 (CC) | A | Unit Economics tab, Ads Manager targets, promotion export | Count active GMV Max targets (net-revenue basis) and promotions below the SKU breakeven ROI (column AA). 2: zero; 0: any |
| T6 | A | Promotion export vs corridor | 2: every promotion inside corridor and margin floor |
| T7 | S | Registration records, readiness checklists | 2: on time with T-21 and T-7 sign-off for last campaign |
| T8 | S | Lift studies | 2: brand or search lift measured for upper-funnel spend |
| T9 | S | CRM tool screenshot | 2: segments and follower vouchers active, repeat rate tracked |
| T10 | M | CRM data-source register | Ask where every CRM contact came from |
| T11 | S | Allocation model | 2: documented split with evidence |
| T12 | S | Test backlog | 2: decision rules agreed before tests |
| T13 | S | Monthly pack | 2: incremental contribution incl. halo and co-funding |

## Evidence request

Send this list, trimmed to the platforms in scope. Ask for exports (CSV or XLSX) over screenshots wherever the platform allows, covering the last 90 days unless noted.

**TikTok Shop / Tokopedia Seller Center:** product list with listing quality; Account Health and Shop Performance Score (4 weekly screenshots); fulfilment performance (LDR, SFCR, VTR); violation record; orders and cancellations with reasons (30 days); returns; reviews; chat performance; LIVE session list with GPM, enter-room rate, CTR, and the LIVE schedule; video analytics by asset (views, 2- and 6-second view rate, product clicks, GPM); product traffic by source with card click-through and page conversion; orders by content type (video, LIVE, product card) and source (brand, affiliate); Shop Tab and search analytics; Affiliate Center (open and target collaborations, rates, Shop Ads rate, sample requests, creator performance); promotions; campaign registrations; customer marketing tool; settlement and finance reports with fee lines (3 months).

**Creative samples:** links to 20 recent videos (brand and affiliate, mixed performance) and mobile screenshots of the top 20 product pages by GMV.

**TikTok Ads Manager and Business Center:** GMV Max campaigns with ROI targets, budgets and consumption; change history; creative library; all other shopping ads campaigns; Business Center users and assets; primary ad account setting.

**Shopee, Lazada and others:** the equivalent product, health, fulfilment, chat, reviews, returns, affiliate, ads and settlement exports.

**Finance:** landed cost per SKU; 3PL or fulfilment invoices; agency, enabler, LIVE studio and KOL invoices and contracts (3 months); sample log with costs; the last month's finance pack and the allocation key, if any.

**Brand documents:** RACI and partner SOWs, contracts, access register, last 4 weekly dashboards, assortment matrix, price corridor, keyword map, content calendar, claims sheet, briefs, run-of-show files, business cases and post-reviews, test logs, compliance register, crisis playbook, surge plan, VoC and QA reports.

## Data handling

- Client data stays in the client's workbook copy and folder. Never put it in the public self-check, the web app, or another client's files.
- Prefer exports the client sends. Log in to a client platform only when the client has given access and asked you to, and only read.
- Use anonymised, aggregated figures for benchmarks only with written client permission.

## Before handing over

- Every scored row has evidence in G, or a score of 1 or less.
- Every Auto score shows its calculation in J.
- C2 and RO4 match the suggested scores in `'Creative Review'!B10:C10`, or J explains why the auditor departed from them.
- `'Store P&L'!B57:B58` both read OK, and every promoted SKU has a Unit Economics row.
- Every CC scored below 2 has an Action Log row.
- The index and stage in your summary match `Scorecard!C13:C14` as read back.