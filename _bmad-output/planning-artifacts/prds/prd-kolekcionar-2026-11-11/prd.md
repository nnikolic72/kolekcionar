---
title: Kolekcionar.app Product Requirements Document (PRD)
status: final
created: 2026-11-11
updated: 2026-11-11
---

# PRD: Kolekcionar.app

## 0. Document Purpose

This Product Requirements Document (PRD) defines the functional requirements, architectural governance, compliance guardrails, and user experience specifications for **Kolekcionar.app**. It serves as the single source of truth for engineering teams, UX designers, system architects, and compliance officers implementing Phase 1 (Platform Owner Showcase & Bootstrap) and Phase 2 (P2P Collector Marketplace).

The document is derived strictly from the hardened specification in [KOLEKCIONAR-REQUIREMENTS.md](file:///Volumes/Kingston%20Nenad%201TB/code/kolekcionar/docs/requirements/KOLEKCIONAR-REQUIREMENTS.md). Technical mechanism details, scraping heuristics, and database schemas are preserved in the accompanying [addendum.md](file:///Volumes/Kingston%20Nenad%201TB/code/kolekcionar/_bmad-output/planning-artifacts/prds/prd-kolekcionar-2026-11-11/addendum.md).

---

## 1. Vision

**Kolekcionar.app** is a specialized ecosystem for modern collectibles (Diecast models, Trading Card Games, Action Figures, and Vinyl Figures) built for the Serbian market with architectural readiness for the Western Balkans. The platform tagline—*"Earn and collect at the same time"*—embodies a dual-purpose mission: providing passionate collectors with a state-of-the-art digital inventory and portfolio tracker, while empowering active enthusiasts and dealers with a trusted, legally compliant marketplace.

Today, collectibles commerce in the region is fragmented across generalist classifieds (e.g., KupujemProdajem), unindexed Facebook groups, and physical exchange fairs (*Berze*). These channels suffer from rampant image theft, counterfeit merchandise, unstandardized condition claims, and non-compliance with domestic trade regulations.

Kolekcionar.app solves this by combining:

1. An authoritative **Master Data Management (MDM)** catalog with strict English nomenclature and cross-script phonetic search.
2. A **100% Free Lifetime Personal Collection & Portfolio Hub** offering multi-currency acquisition tracking, historical National Bank of Serbia (NBS) FX pegging, and unrealized profit margin calculations.
3. A **Classified Reservation & Direct Settlement Marketplace** that fully complies with the Serbian Commerce Law (*Zakon o trgovini*) via recurring monthly seller memberships and direct P2P fulfillment.
4. An **Asynchronous AI Condition & Anti-Fraud Advisory Engine** using perceptual hashing (pHash), visual embeddings (CLIP), and EXIF inspection to eliminate stolen photos and flag counterfeit listings.
5. A verified **Community Events Notice Board (*Berze*)** connecting online collectors to physical meetups.

---

## 2. Target User

### 2.1 Jobs To Be Done (JTBD)

- **Functional (Collector)**: "When I acquire a new collectible or bundle lot, I want to record its purchase price, currency, storage location, and condition against an authoritative catalog record, so I have a single organized view of my collection's total value and appreciation in Serbian Dinars."
- **Functional (Seller)**: "When I want to downsize my collection or run a collectibles business, I want to list items quickly linked to verified master data, have AI certify my photos and condition, and handle reservations without dealing with trolls or commission escrow hurdles."
- **Functional (Buyer)**: "When I hunt for specific variants or mint-condition cards, I want to search across Latin, Cyrillic, or phonetic terms, inspect verified authentic photos with AI condition estimates, and commit to purchase with guaranteed atomic reservations."
- **Emotional & Social**: "I want to showcase my personal binders and displays to the collector community while keeping my acquisition costs and profit margins strictly confidential."

### 2.2 Non-Users (v1 / Phase 1 & 2)

- **Generalist Goods Sellers**: Individuals selling electronics, clothing, auto parts, or miscellaneous household items.
- **Cross-Border Escrow Shoppers**: International buyers seeking multi-currency PayPal or Stripe escrow checkout with automated cross-border customs declarations.
- **Unverified Anonymous Traders**: Users refusing domestic mobile SMS phone verification.

### 2.3 Key User Journeys

#### UJ-1: Marko Organizes & Tracks Portfolio Appreciation

- **Persona + Context**: Marko, a diecast collector in Belgrade tracking over 450 Hot Wheels castings.
- **Entry State**: Authenticated on mobile web; opens his personal collection dashboard.
- **Path**:
  1. Marko taps "+ Add Item" and searches for `Нисан Скајлајн` in Cyrillic.
  2. The cross-script search engine transliterates the query and returns master item *"Hot Wheels Boulevard - Nissan Skyline GT-R (R34) - #71"*.
  3. He selects the 2020 Premium variant, enters an acquisition price of `€15.00` bought at *Berza Ušće*, and selects condition *"Carded / Near Mint"*.
  4. The system automatically fetches the historical NBS middle exchange rate for the purchase date and pegs the base cost at `1,760 RSD`.
- **Climax**: Marko views his portfolio summary showing his unrealized profit margin (+85% in RSD) calculated dynamically against the catalog's fair market value benchmark.
- **Resolution**: He generates a public showcase link (`kolekcionar.app/u/marko-bg/hw-collection`) to share in a Discord group; private purchase price and margins are masked.

#### UJ-2: Stefan Lists a Rare Pokémon Card for Sale (Phase 2 P2P)

- **Persona + Context**: Stefan, an active TCG collector in Novi Sad with an active "Collector Pro" subscription.
- **Entry State**: Authenticated on desktop; navigating from his Personal Collection "Trade Pile".
- **Path**:
  1. Stefan selects *"Charizard #4 Base Set Unlimited"* from his owned inventory and clicks "Create Listing".
  2. He uploads 3 high-resolution phone photos (front, back, holo angle) and sets his asking price to `35,000 RSD`.
  3. The client receives an optimistic confirmation with status `Pending AI Verification` (SLA < 60s).
  4. In the background, the worker confirms the photos do not match scraped web images (pHash/CLIP) and checks EXIF data. The AI engine evaluates corner wear and edge whitening, issuing an AI Condition Score of `8/10 (Minor silvering on top edge)`.
- **Climax**: The listing automatically transitions to `Active` with an official "AI Advisory Badge" displayed alongside Stefan's declared condition.
- **Resolution**: Stefan receives an in-app notification that his card is live in the public marketplace.

#### UJ-3: Milica Reserves and Purchases a Diecast Model

- **Persona + Context**: Milica, searching for a specific Matchbox Superfast model for her husband's birthday.
- **Entry State**: Authenticated with a verified Serbian mobile number (`+381 64...`); browsing on mobile.
- **Path**:
  1. Milica finds an active listing for `3,200 RSD` offered by a verified seller.
  2. She clicks "Reserve Item" (`Rezerviši`).
  3. The database executes an atomic status transition from `Active` to `Reserved`. A 7-day fulfillment TTL is initialized.
  4. Milica and the seller are placed into an end-to-end platform order message thread. Milica's full address and phone number are masked until the seller clicks "Confirm & Accept Order".
  5. The seller accepts fulfillment; Milica's Post Express shipping details are revealed to the seller.
  6. The seller ships the package via Post Express *pouzećem* (Cash on Delivery) and enters the tracking code `PE123456789RS`, transitioning the listing to `InTransit`.
- **Climax**: Milica inspects the delivered parcel at her door, pays the courier `3,200 RSD` in cash, and marks the transaction `Completed` on the platform.
- **Resolution**: Both Milica and the seller unlock the ability to submit a 1-to-1 rating (`Pozitivna`), increasing each other's trust score.

#### UJ-4: Nenad Moderates Ingestion & Scraper Queues (Admin Back-Office)

- **Persona + Context**: Nenad, platform owner and catalog administrator.
- **Entry State**: Authenticated into `/admin` with superadmin privileges.
- **Path**:
  1. Nenad opens the **Change Request Queue** to review batch output from the Hot Wheels Fandom wiki scraper.
  2. He reviews 42 newly identified 2026 mainline castings with side-by-side attribute diffs.
  3. An unrecognized attribute `tampo_designer` is routed to the **Unmapped Attribute Queue**; Nenad maps it as an extensible text attribute for the Diecast taxonomy.
  4. He clicks "Approve Batch", instantly promoting the items into the active production MDM catalog without service disruption.
- **Climax**: Collectors across the platform can immediately link their physical cars to the newly approved master items.

---

## 3. Glossary

The following domain terms must be used verbatim across all requirements, UI copy, and downstream artifacts:

| Term | Definition |
| :--- | :--- |
| **Master Data Management (MDM)** | The centralized system of record defining collectible taxonomy, canonical entities, brand hierarchies, and category attributes. |
| **Tier 1 (Master Item)** | The abstract design blueprint of an item (e.g., casting model or card base design), independent of commercial releases. |
| **Tier 2 (Variant / Release)** | A specific commercial production run or market release of a Master Item (distinguished by year, series, card finish, or packaging). |
| **Tier 3 (User Lot Instance)** | A physical copy of a Variant owned by an individual collector, carrying private purchase data, condition, photos, and notes. |
| **Public Showcase** | A shareable, read-only URL representing a user's curated collection where acquisition costs, source notes, and margins are strictly stripped. |
| **Classified Reservation Model** | A transaction pattern compliant with *Zakon o trgovini* where buyers atomically reserve an item, and financial/logistical settlement is executed directly between parties off-escrow. |
| **Atomic Reservation Guard** | A database-level concurrency safeguard ensuring an item can only be committed to by one buyer at any given millisecond. |
| **Phased Contact Disclosure** | The privacy rule withholding buyer delivery address and telephone from the seller until the seller explicitly confirms order fulfillment. |
| **Anti-Zombie TTL** | An automated 7-day expiration timer that cancels unfulfilled reservations and restores items to active status if the seller fails to dispatch. |
| **In-Flight Order Grace Period** | The operational state protecting buyers and sellers when a seller subscription lapses, maintaining active management for existing `Reserved` and `InTransit` orders until terminal settlement. |
| **pHash (Perceptual Hash)** | A fingerprint algorithm used to identify identical or recompressed images across platform listings and scraped web libraries. |
| **CLIP Embedding** | A multi-modal visual vector representation used to detect crop-, rotation-, and filter-invariant image plagiarism. |
| **AI Advisory Badge** | A non-binding, automated visual score (1–10) and defect summary generated by the computer vision pipeline displayed on public listings. |
| **e-Fiscalization (e-Fiskalizacija)** | Compliance with the Serbian *Zakon o fiskalizaciji* requiring real-time registration of fiscal invoices via SUF/ESIR/PFR with Tax Authority QR code issuance. |
| **Berza (Collector Fair)** | An in-person community trade show or physical meetup indexed on the platform events notice board. |

---

## 4. Features & Functional Requirements

### 4.1 Master Data Management (MDM) & Ingestion Governance

**Description:** Provides an authoritative, 3-tier catalog across modern collectible categories (Diecast, TCG, Action Figures, Funko). Scrapers ingest data into a staging queue where administrators curate and merge records. Realizes **UJ-1, UJ-4**.

#### FR-1: 3-Tier Entity Hierarchy

The system must enforce a strict relational separation between Tier 1 (Master Item), Tier 2 (Variant/Release), and Tier 3 (User Lot Instance).

- **Consequences (testable):**
  - Attempting to instantiate a Tier 3 User Lot without a parent Tier 2 Variant ID returns `HTTP 422 Unprocessable Entity`.
  - Updating a Tier 1 attribute (e.g., casting debut year) cascades metadata updates to associated Tier 2 records without altering private Tier 3 lot values.

#### FR-2: Strict English Nomenclature in Master Records

The catalog database must store official entity titles, brand names, series numbers, and attribute keys/values exclusively in English.

- **Consequences (testable):**
  - Creating or scraping a master entity enforces validation rejecting non-ASCII localized brand names (e.g., rejecting translated casting names).

#### FR-3: Scraper Governance & Ingestion Rate Limiting

All automated catalog extraction crawlers must respect `robots.txt`, prioritize documented public APIs where available, and strictly throttle requests to a maximum rate of 1 request per 2.0 seconds per remote host.

- **Consequences (testable):**
  - Scraping workers log HTTP headers and verify request timestamps; consecutive requests to the same host within < 2000ms trigger a task execution delay.

#### FR-4: Change Request & Staging Ingestion Gate

Scraped data must never write directly to production tables; all incoming records must deposit into a persistent Change Request Queue.

- **Consequences (testable):**
  - Raw scraper output has status `Staged`; records are invisible to public search and user collection pickers until approved in `/admin`.

#### FR-5: Unmapped Attribute Routing

If scraped data contains attribute keys not present in the target category taxonomy, the ingestion pipeline must flag the record and route unrecognized fields to the Unmapped Attribute Queue.

- **Consequences (testable):**
  - Ingestion jobs containing unmapped keys complete with status `Partial Success / Review Required` without aborting mapped fields.

#### FR-6: Public Catalog Image Provenance Restriction

Third-party images obtained via web scraping must be restricted exclusively to internal `/admin` reference. Public catalog thumbnails must only be sourced from:

1. Community-verified user uploads approved by an Admin.
2. Official manufacturer media kit assets with documented fair-use notices.
3. Neutral SVG vector category silhouettes.

- **Consequences (testable):**
  - Public API responses for master items return only approved image URLs or the default vector placeholder; scraped URLs never leak to public endpoints.

---

### 4.2 Personal Collection & Portfolio Management Hub

**Description:** A 100% free lifetime utility for registered collectors to catalog items, manage custom binders, track multi-currency acquisition costs, and analyze unrealized portfolio margins pegged to historical NBS exchange rates. Realizes **UJ-1**.

#### FR-7: Free Lifetime Collection Tier

The system must grant registered users unlimited collection tracking capacity without requiring payment, trial periods, or seller memberships.

- **Consequences (testable):**
  - An unsubscribed user can add an arbitrary number of Tier 3 lots to their personal collection without triggering paywalls.

#### FR-8: Multi-List Organization & Wishlist Match Alerts

Users can categorize lots across standard lists (`Owned`, `Wishlist`, `Trade/Sell`) and arbitrary custom binders/tags. Adding an item to `Wishlist` automatically registers a notification trigger.

- **Consequences (testable):**
  - When an `Active` marketplace listing is published that matches an item in a user's `Wishlist`, an in-app and email alert is dispatched within 120 seconds.

#### FR-9: Lot-Level Acquisition & Historical NBS FX Pegging

Users can record lot acquisitions in RSD, EUR, or USD. For foreign currency entries, the system must capture the acquisition date and fetch the official National Bank of Serbia (NBS) middle exchange rate. If the acquisition date falls on a weekend, statutory holiday, or non-banking day when the NBS does not publish official exchange rates, the system must deterministically fall back to the most recent preceding published NBS middle exchange rate (e.g., the preceding Friday rate). The system must persist both `acquisition_date` and `effective_rate_date` alongside the converted base RSD cost.

- **Consequences (testable):**
  - Creating a lot with `cost = 10 EUR` on `2023-05-07` (Sunday) looks back to the official published rate from Friday `2023-05-05`, stores `effective_rate_date = '2023-05-05'`, and calculates the base cost in RSD without failing.

#### FR-10: Dynamic Unrealized Profit Calculation

The system must dynamically calculate unrealized profit margin in base currency (RSD) against the current benchmark Fair Market Value (FMV), isolating true asset appreciation from currency fluctuations.

- **Consequences (testable):**
  - The portfolio view displays: `Unrealized Margin = Current FMV (RSD) - Historical Cost (RSD)`.

#### FR-11: Public Showcase Isolation

Users can generate a public read-only showcase URL for their collection. Private acquisition prices, sources, storage notes, and profit margins must be filtered out of public views and responses.

- **Consequences (testable):**
  - Querying `/api/v1/showcase/:username` returns item metadata, condition, and public photos, but strictly omits `acquisition_cost`, `provenance`, and `profit_margin`.

---

### 4.3 Multi-Tier Seller Subscription & Billing Engine

**Description:** Manages recurring monthly subscriptions in Serbian Dinars (RSD) for sellers, integrated with domestic payment gateways and Serbian e-fiscalization services. Realizes **UJ-2**.

#### FR-12: Volume-Based Seller Subscription Tiers

The platform must support volume-based recurring monthly seller subscriptions billed in RSD:

- **Starter / Hobbyist**: Up to 25 concurrently active listings `[ASSUMPTION: 590 RSD / month]`.
- **Collector / Pro**: Up to 100 concurrently active listings `[ASSUMPTION: 1,490 RSD / month]`.
- **Dealer / Power**: Unlimited concurrently active listings `[ASSUMPTION: 2,990 RSD / month]`.
- **Consequences (testable):**
  - A seller on the Starter tier attempting to publish a 26th active listing receives `HTTP 403 Forbidden` with a prompt to upgrade.

#### FR-13: Domestic Payment Gateway & NBS IPS QR Integration

Subscription billing must process through certified domestic payment gateways (ChipCard, AllSecur, Banca Intesa, or UniCredit e-commerce) or National Bank of Serbia Instant Payment System (NBS IPS Skeniraj / IPS Pokaži) QR codes.

- **Consequences (testable):**
  - Selecting IPS payment renders an NBS-compliant QR code with standard payload attributes (`K:PR|V:01|...`). International processors (Stripe/PayPal) are rejected in Phase 1 & 2.

#### FR-14: Serbian e-Fiscalization Compliance & Offline Resilience (ESIR/SUF/PFR)

Every successful domestic subscription charge (and Phase 1 retail purchase) must trigger fiscalization with an authorized Serbian e-fiscalization software service (ESIR / PFR):

1. **Synchronous PFR Execution**: Under normal connectivity, the system signs the fiscal invoice via local/cloud PFR, verifies the Tax Authority QR code URL and PFR number, attaches it to the billing record, and emails the PDF receipt.
2. **Offline Resilience (PFR Unreachable)**: If the Ministry of Finance SUF/PFR cloud endpoint is down or times out (> 3.0s), the payment capture must **not** be rolled back or aborted. The system must issue a locally signed offline fiscal receipt utilizing the platform's authorized digital certificate/security element (*bezbednosni element*), grant the subscription immediately, and enqueue the invoice into a durable dead-letter queue (DLQ) for automated synchronization to the Tax Authority SUF portal within the statutory 5-day window.

- **Consequences (testable):**
  - Simulating an HTTP 504 Gateway Timeout on the external SUF portal during a successful IPS payment triggers the offline fallback: the subscription activates immediately (`HTTP 200`), an offline fiscal record is recorded, and the invoice is flagged `PendingSUFSync`.

#### FR-15: Subscription Lapse, Downgrade & In-Flight Order Grace Period

The billing engine handles both renewal lapse and voluntary plan downgrades:

1. **Subscription Lapse**:
   - All public listings in status `Active` immediately transition to `Paused` and are hidden from search.
   - Any listing in status `Reserved`, `InTransit`, or `Disputed` retains complete functionality until final settlement.
   - The seller retains complete order fulfillment, messaging, and courier tracking capabilities.
2. **Voluntary Tier Downgrade (Quota Compression)**:
   - When a seller switches to a lower tier with fewer allowed listings (e.g., from Dealer Power with 150 items to Collector Pro with 100), the seller enters a **48-Hour Curation Window** to manually select which listings remain `Active`.
   - If the seller does not make a selection within 48 hours, the system deterministically keeps the top `N` listings ranked by most recent view count and updates the excess listings to `Paused`.
   - In-flight orders remain entirely unaffected during and after the transition.

- **Consequences (testable):**
  - A lapsed seller cannot publish new listings or unpause items, but can confirm fulfillment and enter courier tracking for an order reserved prior to subscription expiration.
  - A seller downgrading from Dealer Power (150 listings) to Collector Pro (100 listings) who takes no action for 48 hours retains their 100 most viewed listings in `Active`, with the remaining 50 safely transitioned to `Paused`.

#### FR-16: One-Click Subscription Renewal Unpause

Upon successful payment of an overdue subscription or plan upgrade, the seller must be provided with a single-click action to restore previously `Paused` listings back to `Active` up to their active tier quota.

- **Consequences (testable):**
  - Triggering bulk unpause updates up to tier limit listings from `Paused` to `Active` in a single atomic batch.

---

### 4.4 Marketplace Listing, Atomic Reservation & P2P Order Settlement

**Description:** Enables buyers to atomically reserve items from subscribed sellers, protects buyer privacy via phased disclosure, and coordinates direct settlement (Post Express COD / Meetup) compliant with *Zakon o trgovini*. Realizes **UJ-3**.

#### FR-17: Listing Creation Prerequisites

To publish a public marketplace listing, a seller must have an active subscription, link the listing to a verified Tier 2 Variant, upload at least 2 original high-resolution photos, and declare an item condition.

- **Consequences (testable):**
  - Submitting a listing without a Tier 2 ID or with fewer than 2 images fails schema validation.

#### FR-18: Multi-Quantity Inventory Support

Listings support discrete quantities (`quantity >= 1`). When a buyer reserves a partial quantity (`requested_qty <= available_qty`), available inventory decrements atomically and a dedicated sub-order record is created.

- **Consequences (testable):**
  - If a listing has `quantity = 3` and a buyer reserves `1`, the listing remains `Active` with `quantity = 2`, and an order record is created for `1` unit. When remaining `quantity` reaches `0`, the listing status transitions to `Reserved`.

#### FR-19: Atomic Concurrency Guard on Reservation

To prevent overselling and duplicate commitments across both single- and multi-unit listings, reservation must execute an atomic conditional decrement:

```sql
UPDATE listings
SET quantity = quantity - :requested_qty,
    status = CASE WHEN quantity - :requested_qty = 0 THEN 'Reserved' ELSE 'Active' END,
    updated_at = NOW()
WHERE id = :id
  AND status = 'Active'
  AND quantity >= :requested_qty
RETURNING id, quantity, status;
```

- **Consequences (testable):**
  - When two concurrent requests target the same listing simultaneously attempting to reserve the last unit, exactly one succeeds (`HTTP 200` returning the created order) and the competing request immediately receives a conflict error (`HTTP 409 Conflict: Insufficient quantity`).

#### FR-20: Phased Contact Disclosure

When an item is reserved:

1. Reserving opens a direct in-platform messaging thread between buyer and seller.
2. Full delivery address and phone number are withheld from the seller.
3. Full shipping details are unlocked to the seller only after the seller clicks "Confirm & Accept Order".

- **Consequences (testable):**
  - Querying order details while `status = 'Reserved'` and `confirmed_by_seller = false` returns masked phone and address fields (`+381 64 *** **12`).

#### FR-21: Anti-Zombie 7-Day Reservation TTL

A reservation has a 7-day time-to-live (TTL). If the seller fails to confirm and dispatch within 7 days (168 hours), the reservation expires automatically (`CancelledBySystem`), and the reserved quantity reverts to the listing's available stock (restoring status to `Active`, or `Paused` if the seller has lapsed).

- **Consequences (testable):**
  - An automated scheduled job identifies reservations with `created_at < NOW() - INTERVAL '7 days'` in unfulfilled state, marks them `CancelledBySystem`, and restores inventory atomically.

#### FR-22: Fulfillment, Courier Tracking & COD Status Lifecycle

The seller marks an accepted order as dispatched by entering an authentic courier tracking number (e.g., Post Express tracking ID `PE...RS`), transitioning the state to `InTransit`.

The system periodically polls the domestic courier API/web tracking endpoint (or enables seller/admin status synchronization) to track terminal states:

1. **Delivered (`Uručen`)**: Transitions order to `Completed` (or unlocks manual buyer confirmation).
2. **Returned / Refused (`Vraćen pošiljaocu` / `Neuručen`)**: When a buyer refuses Cash on Delivery acceptance or fails to claim the parcel within the statutory Post Express 5-day holding period (*ležašina*), the order enters terminal status `CourierReturned`.
3. The seller can re-list the returned item with a single click without deducting tier quota.

- **Consequences (testable):**
  - Transitioning an order to `InTransit` requires a valid alphanumeric tracking reference.
  - An order marked `CourierReturned` frees the seller from fulfilling the reservation and marks the transaction as failed delivery.

#### FR-23: 14-Day Delivery Auto-Completion Guard

If an order remains in `InTransit` for 14 calendar days without a dispute, courier return scan, or manual confirmation, the system automatically transitions the state to `Completed` and unlocks feedback.

- **Consequences (testable):**
  - InTransit orders older than 14 days without an open dispute ticket or return scan automatically update to `Completed` during daily cron runs.

#### FR-24: Phase 1 Platform Owner Retail Showcase

In Phase 1, only the platform owner (Admin) can list and sell direct inventory. The platform provides integrated domestic card checkout and automated Post Express API shipping label generation exclusively for Phase 1 admin orders.

- **Consequences (testable):**
  - In Phase 1, non-admin accounts do not have access to listing creation endpoints or payment checkout APIs.

---

### 4.5 AI Condition Grading & Multi-Vector Anti-Fraud Engine (Phase 2)

**Description:** Asynchronous image analysis pipeline evaluating uploaded listing photos for plagiarism, internet stock images, and physical condition defects. Realizes **UJ-2**.

#### FR-25: Two-Stage Asynchronous Optimistic Listing Submission

Photo analysis runs asynchronously via a two-stage background pipeline. Upon photo upload, the listing is saved immediately with status `Pending AI Verification` (SLA < 60s):

1. **Stage 1 (Immediate CPU Gate, < 2s)**: Fast perceptual hashing (pHash) and file integrity checks execute on standard CPU workers to detect exact or near-exact image duplicates against scraped web catalogues.
2. **Stage 2 (Deep Visual & Condition Worker, < 60s)**: Quantized multi-modal visual embeddings (CLIP) and condition scoring evaluate card corners, edge wear, blister dents, and package warping.

- **Consequences (testable):**
  - Listing submission returns `HTTP 202 Accepted` in < 1.0 second; public search excludes listings until status reaches `Active`.

#### FR-26: Multi-Vector Plagiarism Detection & Messaging EXIF Normalization

The pipeline evaluates submitted images against scraped reference archives and platform listings using:

1. **pHash**: Exact and near-duplicate hamming-distance matching (hamming distance <= 5 flags exact match).
2. **CLIP Visual Embeddings**: Cosine similarity check invariant to cropping, rotation, border additions, and color filtering (cosine similarity > 0.92 flags stock image).
3. **EXIF Inspection**: Evaluates camera metadata. Stripped or missing EXIF (characteristic of compression via messaging channels such as Viber or WhatsApp) is treated as a **neutral indicator** and does **not** trigger fraud escalation on its own. Only contradictory synthetic EXIF (e.g. mismatched sensor dimensions and camera headers) or direct visual matches escalate risk.

- **Consequences (testable):**
  - Submitting a genuine mobile photo with stripped EXIF (forwarded via Viber) completes with low fraud risk (< 20%) and does not block publication.
  - Submitting an image matching a scraped web thumbnail with cosine similarity > 0.92 immediately flags the upload as a stock photo.

#### FR-27: Automated Publication vs. Fraud Escalation

- **Low Risk (< 70% risk)**: Listing status transitions to `Active`. An AI Advisory Badge is attached displaying estimated condition (1–10) and identified minor defects.
- **High Risk (>= 70% counterfeit risk or plagiarism match)**: Listing transitions to `Pending Admin Review`, remains hidden from public search, and routes to `/admin` Listing Moderation Queue.
- **Consequences (testable):**
  - Listings with counterfeit probability = 0.75 are flagged and held in admin queue without appearing in search results.

---

### 4.6 Trust, Reputation & Anti-Abuse Feedback System

**Description:** Protects the community from bad actors, shill reviews, and extortion through verified-transaction ratings, SMS verification, COD refusal penalties, and admin dispute arbitration. Realizes **UJ-3**.

#### FR-28: Feedback Unlocking Exclusively on Completed Orders

Only transactions that have reached terminal `Completed` status unlock feedback submission. Rating options: Positive (`Pozitivna`), Neutral (`Neutralna`), Negative (`Negativna`) with mandatory comment (minimum 10 characters).

- **Consequences (testable):**
  - Submitting feedback on a listing in `Reserved` or `InTransit` returns `HTTP 403 Forbidden`.

#### FR-29: Anti-Extortion, COD Ghosting Guard & Serial Refusal Penalties

If a reservation is cancelled prior to dispatch due to non-payment, courier return, or mutual agreement, feedback is permanently disabled for that order. Buyers cannot submit negative ratings for unfulfilled transactions:

1. Orders in `CancelledBySystem`, `CancelledByMutual`, or `CourierReturned` return `feedback_eligible = false`.
2. **Serial COD Refusal Guard**: Any buyer account accumulating two (2) or more `CourierReturned` orders (uncollected or refused Post Express COD parcels) within a rolling 90-day window automatically receives an automated 30-day reservation freeze (`trading_restricted_until = NOW() + 30 days`).
3. **Fulfillment Trust Ratio**: Profiles display a "Successful Delivery Rate" percentage based on completed vs. refused COD deliveries, allowing sellers to evaluate reservation credibility.

- **Consequences (testable):**
  - A user with 2 `CourierReturned` deliveries attempting a 3rd reservation receives `HTTP 403 Forbidden: Account temporarily restricted due to uncollected Cash-on-Delivery parcels`.

#### FR-30: Mandatory Serbian Mobile Phone Verification

Users must verify an authentic Serbian mobile number (`+381`) via SMS OTP before they can reserve items, create listings, or leave reviews. Virtual VOIP numbers are blocked via carrier prefix validation.

- **Consequences (testable):**
  - Attempting to initiate reservation with an unverified account returns `HTTP 403` with a phone verification challenge.

#### FR-31: 48-Hour Phone Number Modification Cooling Period

When a user updates their verified phone number, their account enters an automated 48-hour security cooling period restricting listing creation and item reservation.

- **Consequences (testable):**
  - Modifying phone number sets `trading_restricted_until = NOW() + 48 hours`.

#### FR-32: Administrative Feedback Arbiter

Administrators can inspect disputed feedback tickets, review complete platform chat transcripts and courier tracking logs, and redact bad-faith, retaliatory, or defamatory ratings.

- **Consequences (testable):**
  - Admin redaction action marks a review as `Redacted`, recalculates the user's aggregate score, and logs the admin ID in audit records.

---

### 4.7 Community Events Notice Board (*Berze*)

**Description:** Public directory of upcoming physical collector fairs and meetups in Serbia (*TC Ušće, Dom Omladine*, regional fairs). Realizes **UJ-1**.

#### FR-33: Moderated Meetup Submission & Venue Verification

Registered users and verified event organizers can submit proposed community meetups. Submissions enter the Event Moderation Queue and require Admin approval before public indexing. Administrators must verify physical venue details, dates, and organizer authenticity to prevent fraudulent or spoofed gathering notices.

- **Consequences (testable):**
  - New event submissions have `status = 'Pending'` and are invisible on the public `/events` page until approved in `/admin`.

#### FR-34: Anti-Spoofing RSVP Actions & Cancellation Alerts

Only authenticated users with verified Serbian mobile numbers (`+381`) can register an RSVP ("Dolazim / I'm attending"). Rate limiting restricts RSVP submissions to prevent automated vote stuffing:

1. **Attendee Count Integrity**: RSVP is strictly 1-per-verified-user per event, preventing artificial attendee inflation.
2. **Cancellation & Reschedule Broadcast**: If an event date, venue, or status transitions to `Cancelled` or `Rescheduled`, the platform immediately dispatches an automated in-app notification and SMS alert to all RSVP-confirmed users.

- **Consequences (testable):**
  - Unverified users clicking RSVP receive a phone verification prompt (`HTTP 403`).
  - An admin marking an event as `Cancelled` automatically triggers an alert broadcast to all linked RSVP user IDs within 60 seconds.

#### FR-35: Statutory Liability Disclaimer Display

Every event detail page must display the standard statutory platform disclaimer stating that Kolekcionar.app is an informational directory and bears no liability for transactions conducted at in-person venues.

- **Consequences (testable):**
  - Event template renders the mandatory legal disclaimer paragraph verbatim (§10.2).

---

### 4.8 Back-Office Admin Operations & Queues

**Description:** Centralized control room for administrators to manage master data, review scraped items, inspect fraud, audit fiscalization, and resolve disputes. Realizes **UJ-4**.

#### FR-36: MDM Entity & Taxonomy Manager

Admins have full CRUD capabilities over Categories, Sub-categories, Manufacturers, Brands, Sub-brands, Dynamic Attribute Definitions, Master Items, and Variants.

- **Consequences (testable):**
  - Admin updates to category attribute definitions validate schema consistency across linked variants.

#### FR-37: Listing Moderation & Fraud Review Queue

Surfaces listings flagged by the AI engine (counterfeit risk > 70%, pHash match, or missing EXIF) with side-by-side visual diffs and one-click actions: Approve, Reject & Request Re-photo, or Ban Seller.

- **Consequences (testable):**
  - Rejecting a flagged listing updates its status to `Rejected`, notifies the seller with specific defect reasons, and archives the decision.

#### FR-38: Audit Trail & e-Fiscal Invoice Registry

Maintains an immutable registry of all issued e-fiscal receipts, subscription billing logs, and admin moderation overrides.

- **Consequences (testable):**
  - All moderation decisions and billing transactions record an un-deletable log entry including timestamp, actor ID, IP address, and payload diff.

---

### 4.9 Localization, Cross-Script & Diacritic Search Architecture

**Description:** Multilingual interface and search engine supporting Serbian Latin, Serbian Cyrillic, and English. Realizes **UJ-1**.

#### FR-39: Multilingual UI Localization

The platform UI, navigation, taxonomy, and legal terms must be localized across:

- Serbian Latin (`sr-Latn`)
- Serbian Cyrillic (`sr-Cyrl`)
- English (`en`)
User language selection persists across sessions.
- **Consequences (testable):**
  - Switching language updates all static UI strings while keeping master collectible names strictly in English.

#### FR-40: Cross-Script, Diacritic-Insensitive & Phonetic Search Pipeline

Search queries must pass through an extensible multi-stage normalization pipeline to handle the complexities of the domestic collectibles market:

1. **Unicode Normalization & Diacritic Stripping**: Maps `č`, `ć`, `ž`, `š`, `đ` to ASCII equivalents `c`, `c`, `z`, `s`, `dj`.
2. **Serbian Digraph Parity**: Ensures equivalent treatment of composite digraphs `dž`, `lj`, `nj`.
3. **Punctuation, Year-Prefix & Scale Normalization**:
   - Normalizes leading apostrophes in vintage vehicle model years (e.g., `'67 Camaro` or `’67` $\leftrightarrow$ `67 Camaro`).
   - Normalizes scale fractions and ratios (e.g., `1/64` $\leftrightarrow$ `1:64`).
   - Strips non-alphanumeric separators in brand and card names (e.g., `Yu-Gi-Oh!` $\leftrightarrow$ `Yugioh`, `G.I. Joe` $\leftrightarrow$ `GI Joe`).
4. **Token-Level Mixed-Script Transliteration**:
   - Analyzes user queries token-by-token, allowing mixed-script queries (e.g., `Hot Wheels Нисан` or `Pokemon Чаризард`) where Cyrillic tokens are transliterated independently while English tokens remain intact.
5. **Phonetic & Fuzzy Transliteration**:
   - Matches local phonetic transliterations to canonical English titles (e.g., `Betmen` $\leftrightarrow$ `Batman`, `Pokemoni` $\leftrightarrow$ `Pokemon`, `Skajlajn` $\leftrightarrow$ `Skyline`).
   - Enforces autocomplete response latency p95 < 150ms under peak load (500 req/sec).

- **Consequences (testable):**
  - Searching `Нисан Скајлајн`, `nisan skajlajn`, or `’67 Камаро 1/64` correctly returns master records indexed under `Nissan Skyline` and `'67 Camaro` (1:64 scale).
  - Searching a mixed query `Hot Wheels Нисан` executes token transliteration without throwing parse errors and matches `Hot Wheels Nissan`.

---

## 5. Non-Goals (Explicit)

The following capabilities are explicitly out of scope for v1 (Phase 1 & Phase 2):

- **In-App Payment Escrow for P2P**: Kolekcionar.app will **not** hold buyer funds in escrow or charge percentage-based transaction fees on P2P marketplace sales, avoiding compliance liabilities under the Serbian Banking & Commerce Laws.
- **Direct Cross-Border Shipping & International Payments**: The platform will not integrate Stripe, PayPal, or automated international courier customs declarations in Phase 1 or 2. Operations are restricted to Serbia (with local courier COD).
- **Automated AI Card Grading Encapsulation (Slabs)**: The platform provides advisory condition scoring only; it does not issue physical graded plastic slabs or guarantee financial appraisal values.
- **Real-Time Auction Bidding Engine**: Real-time live bidding, countdown auctions, and sniper bid engines are deferred to future phases; Phase 1 and 2 focus strictly on fixed-price classified reservations.

---

## 6. MVP Scope (Phasing)

### 6.1 Phase 1 — Platform Owner Showcase & Catalog Bootstrap (In Scope)

- Core MDM Catalog with 3-tier hierarchy and dynamic attributes (Diecast, TCG, Action Figures, Funko).
- Web scrapers for Hot Wheels Fandom wiki and card databases with Change Request Queue.
- Free Personal Collection & Portfolio Hub with multi-currency tracking (RSD, EUR, USD) and historical NBS FX pegging.
- Platform Owner (Admin) retail storefront with domestic card processing and Post Express API integration.
- e-Fiscalization integration (ESIR/SUF/PFR) with verifiable QR receipt generation for admin retail sales.
- Multilingual UI (sr-Latn, sr-Cyrl, en) with cross-script diacritic search pipeline.
- Community Events Notice Board (*Berze*) directory and moderated submissions.
- Admin Console for MDM management, scraping review, and event approval.

### 6.2 Phase 2 — Multi-Seller P2P Marketplace Launch (In Scope)

- Volume-based recurring seller monthly subscriptions (Starter, Collector Pro, Dealer Power) billed in RSD.
- NBS IPS QR Code (Skeniraj / Pokaži) subscription payment integration.
- P2P Classified Reservation state machine with atomic reservation guard and anti-zombie 7-day TTL.
- Phased contact disclosure protecting buyer shipping information.
- Post Express Cash on Delivery (pouzećem) workflow and courier tracking number entry.
- Asynchronous AI Condition & Anti-Fraud Engine (pHash, CLIP, EXIF) with SLA < 60s.
- Automated Listing Moderation Queue for AI-flagged listings.
- Trust, reputation, and anti-extortion feedback system with mandatory SMS OTP phone verification (`+381`).
- Subscription lapse handling with In-Flight Order Grace Period and one-click bulk unpause.

### 6.3 Out of Scope (Deferred to Phase 3+)

- Regional marketplace expansion with localized courier integrations for Croatia, Bosnia & Herzegovina, and Montenegro.
- Advanced automated pricing predictive models using international secondary market trend vectors.
- Community forum and integrated collector social feed.
- Native mobile applications (iOS / Android) — initial delivery is responsive mobile-first web.

---

## 7. Success Metrics & Counter-Metrics

### Primary Metrics

- **SM-1 (Catalog Depth)**: Over 15,000 verified master variants cataloged across Diecast and TCG within 90 days of launch. *Validates FR-1, FR-3, FR-4.*
- **SM-2 (Collector Engagement)**: Over 1,000 active registered users tracking an average of >= 25 items in their personal collection within 60 days of Phase 1 launch. *Validates FR-7, FR-8, FR-9.*
- **SM-3 (P2P Conversion Rate)**: >= 60% of reserved items successfully reach `Completed` status within the 7-day fulfillment window during Phase 2. *Validates FR-19, FR-20, FR-21.*
- **SM-4 (Subscription Revenue)**: >= 150 active paying sellers subscribed across tiers within 90 days of Phase 2 launch. *Validates FR-12, FR-13, FR-14.*

### Secondary Metrics

- **SM-5 (Search Efficiency)**: Autocomplete search response latency p95 < 150ms across Cyrillic and Latin queries. *Validates FR-40.*
- **SM-6 (AI Verification Speed)**: Asynchronous listing verification turnaround SLA p90 < 60 seconds. *Validates FR-25.*

### Counter-Metrics (Do Not Optimize)

- **SM-C1 (Listing Volume vs. Catalog Quality)**: Do not increase total published listings at the expense of unverified master data or unreviewed stock images. AI fraud thresholds must remain strictly at >= 70%. *Counterbalances SM-1 and SM-4.*
- **SM-C2 (Monetization Friction vs. Collector Growth)**: Never gate personal collection tracking, portfolio margin analytics, or wishlist alerts behind a paid subscription. *Counterbalances SM-4.*

---

## 8. Resolved Architectural Decisions (Hardened)

All prior open questions have been evaluated by engineering, legal, and operational stakeholders and definitively resolved:

1. **Domestic Courier API Direct Integration (P2P Sellers)**:
   - **Decision**: In Phase 2, P2P sellers **must manually enter** their Post Express courier tracking reference (`PE...RS`) upon parcel dispatch.
   - **Rationale**: Generating shipping labels (*adresnice*) under platform corporate credentials creates joint consignor liability for damaged/prohibited goods and bills returned package postage to the platform. Under *Zakon o trgovini*, keeping shipping directly between the private seller and buyer preserves the platform's classification as a non-transport classified listing service. Direct platform label generation remains strictly confined to Phase 1 Platform Owner retail inventory.
2. **NBS IPS QR Dynamic Payload Generation**:
   - **Decision**: IPS QR codes for seller subscriptions must be generated dynamically via the **Merchant Acquirer Payment Gateway API** (e.g., ChipCard / Banca Intesa / AllSecur) rather than offline local specification generation.
   - **Rationale**: Acquirer-generated payloads cryptographically tie the IPS reference to the user's billing session and emit authenticated server-to-server webhooks upon real-time bank settlement, eliminating the need for fragile offline bank statement reconciliation (MT940/CAMT.053).
3. **SMS Gateway Provider Selection**:
   - **Decision**: **Infobip** is designated as the primary domestic SMS aggregator for OTP verification, with AWS SNS maintained as a dormant secondary failover.
   - **Rationale**: Infobip possesses direct SS7 tier-1 peering with all three Serbian mobile network operators (Telekom Srbija/MTS, Yettel, and A1 Serbia), guaranteeing > 99.5% delivery within 10 seconds, verified registered Alphanumeric Sender ID (`KOLEKCIONAR`), and native carrier prefix validation that filters VOIP/virtual blocks at the gateway level.

---

## 9. Assumptions Index

- `[ASSUMPTION: Tier Pricing]` Starter tier is assumed at 590 RSD/month (up to 25 listings), Collector Pro at 1,490 RSD/month (up to 100 listings), and Dealer Power at 2,990 RSD/month (unlimited listings).
- `[ASSUMPTION: Carrier Prefix Filtering]` Serbian mobile phone verification validates carrier network codes (`060`, `061`, `062`, `063`, `064`, `065`, `066`, `0677`, `068`, `069`) and rejects known VOIP/virtual ranges.
- `[ASSUMPTION: Post Express COD Settlement]` In Phase 2, payment collection for P2P goods is settled directly at delivery via Post Express courier cash-on-delivery (*otkupnina*), which the courier deposits into the seller's bank account or pays in cash.
- `[ASSUMPTION: AI Model Deployment]` Two-stage architecture: Stage 1 pHash and file integrity evaluation run on lightweight CPU workers (< 100ms per image), while Stage 2 quantized CLIP embeddings run on containerized background worker nodes with optional GPU or CPU-quantized ONNX acceleration, maintaining average batch inference under 10 seconds per listing without requiring expensive 24/7 dedicated GPU instances.

---

## 10. Adapt-In Sections

### 10.1 Cross-Cutting Non-Functional Requirements (NFRs)

#### Performance & Responsiveness

- **NFR-1**: Largest Contentful Paint (LCP) must be under 1.5 seconds on desktop and 4G mobile connections.
- **NFR-2**: Autocomplete search endpoint response latency p95 must remain under 150ms under peak load (500 req/sec).
- **NFR-3**: Uploaded user photos must be processed asynchronously and compressed into modern WebP/AVIF formats with responsive breakpoints (thumbnail 150px, card 400px, high-res zoom 1600px).

#### Security & Data Protection (ZZPL / GDPR)

- **NFR-4**: Compliance with the Serbian Data Protection Law (*Zakon o zaštiti podataka o ličnosti* - ZZPL).
- **NFR-5**: User passwords must be hashed using `Argon2id` (or `bcrypt` with work factor 12+).
- **NFR-6**: Sensitive personal identifiable information (PII) such as phone numbers, home delivery addresses, and private lot acquisition costs must be encrypted at rest using AES-256-GCM.
- **NFR-7**: Rate limiting enforced on all authentication endpoints, search queries (max 60 req/min/IP), SMS verification requests (max 3 attempts per hour), and RSVP actions.

#### Reliability, Backups & Disaster Recovery

- **NFR-8**: Daily automated PostgreSQL backups with point-in-time recovery (PITR) retained for 30 calendar days.
- **NFR-9**: Multi-region redundant cloud object storage with versioning enabled for all collection and listing imagery.
- **NFR-10**: Immutable write-only audit logs for all e-fiscal receipt generation, subscription status mutations, and administrative queue approvals.

---

### 10.2 Compliance, Legal & Fiscalization Framework

#### Serbian Commerce Law (*Zakon o trgovini*)

To operate lawfully without holding an expensive electronic money institution (EMI) license or acting as an unauthorized escrow agent, the Phase 2 marketplace operates strictly on a **Classified Reservation & Direct Settlement Model**:

- Kolekcionar.app charges sellers a fixed recurring membership fee for access to catalog listing infrastructure.
- The platform never collects, holds, or processes the purchase price of P2P goods.
- Physical delivery and financial settlement occur directly between the buyer and seller via Post Express COD (*pouzećem*), bank wire transfer, or in-person cash handover.

#### Serbian e-Fiscalization Law (*Zakon o fiskalizaciji*)

- For all domestic subscription payments and Phase 1 retail inventory sales, the platform integrates with an authorized ESIR / PFR fiscal service.
- The system immediately verifies that fiscal invoices contain:
  1. Transaction amount in RSD with applicable VAT (PDV) breakdown.
  2. Electronic signature and unique invoice identifier (PFR broj).
  3. Tax Authority QR code link allowing real-time verification on the official Ministry of Finance portal.
- Fiscal receipts are emailed to subscribers and stored in the user billing history.

#### Physical Event Statutory Liability Disclaimer

The following disclaimer must appear prominently on all physical collector fair (*Berza*) pages:
> *"Kolekcionar.app je informativni adresar i nije organizator niti garant fizičkih okupljanja. Kolekcionar.app ne snosi pravnu niti materijalnu odgovornost za interakcije, transakcije ili fizičku razmenu predmeta obavljenu van platforme na navedenim lokacijama."*

---

### 10.3 Information Architecture & Platform Surfaces

The platform encompasses three primary surfaces:

1. **Public Collector Web (Mobile-First Responsive)**:
   - `/`: Home, featured showcase, recent additions, upcoming *Berze*.
   - `/catalog`: 3-Tier MDM navigation (Categories, Brands, Series, Master Items, Variants).
   - `/marketplace`: Active listings filterable by category, condition, price, and AI badge.
   - `/events`: Upcoming physical meetup notice board (*Berze*).
   - `/u/:username`: Public collector showcase.
2. **Personal Collector & Seller Dashboard**:
   - `/my/collection`: Multi-list portfolio hub (`Owned`, `Wishlist`, `Trade Pile`, Custom Binders) with dynamic profit margin analytics.
   - `/my/listings`: Active, Paused, Reserved, and In-Flight orders.
   - `/my/orders`: Purchase reservations, courier tracking, and order messaging.
   - `/my/subscription`: Plan tier management, payment method, and e-fiscal receipts.
3. **Back-Office Admin Console (`/admin`)**:
   - `/admin/mdm`: Entity and taxonomy CRUD.
   - `/admin/change-requests`: Web scraper staging queue and attribute mapper.
   - `/admin/moderation`: AI-flagged listing review queue.
   - `/admin/users`: Subscription audits, phone verification status, and security bans.
   - `/admin/disputes`: Feedback arbiter and transaction dispute logs.
   - `/admin/events`: Community fair submissions review.
