# Kolekcionar.app PRD — Technical Addendum & Downstream Specifications

This addendum preserves technical mechanisms, scraping architectures, state machines, and legal rationale extracted from `KOLEKCIONAR-REQUIREMENTS.md` for downstream architecture, UX design, and engineering implementation.

---

## 1. Technical Data Architecture

### 1.1 3-Tier MDM Schema Mapping
```
Tier 1: Master Item / Base Design
  ├── id (UUID)
  ├── category_id (FK -> categories)
  ├── sub_category_id (FK -> sub_categories)
  ├── manufacturer_id (FK -> manufacturers)
  ├── brand_id (FK -> brands)
  ├── sub_brand_id (FK -> sub_brands)
  ├── canonical_name (VARCHAR, strict English)
  ├── debut_year (SMALLINT)
  └── dynamic_attributes (JSONB)

Tier 2: Variant / Commercial Release
  ├── id (UUID)
  ├── master_item_id (FK -> master_items)
  ├── release_year (SMALLINT)
  ├── series_number (VARCHAR)
  ├── tampo_deco (TEXT)
  ├── wheel_type (VARCHAR)
  ├── base_material (ENUM: plastic, metal)
  ├── card_finish (ENUM: regular, foil, holo, reverse_holo)
  ├── packaging_type (ENUM: carded_blister, boxed, loose)
  ├── official_image_url (VARCHAR, approved only)
  └── dynamic_attributes (JSONB)

Tier 3: User Lot Instance
  ├── id (UUID)
  ├── user_id (FK -> users)
  ├── variant_id (FK -> variants)
  ├── list_type (ENUM: owned, wishlist, trade, custom)
  ├── acquisition_price (DECIMAL(12, 2), private)
  ├── acquisition_currency (ENUM: RSD, EUR, USD, private)
  ├── acquisition_date (DATE, private)
  ├── nbs_exchange_rate (DECIMAL(10, 4), private)
  ├── acquisition_source (VARCHAR, private)
  ├── storage_location (VARCHAR, private)
  ├── private_notes (TEXT, private)
  ├── declared_condition (VARCHAR)
  └── public_visible (BOOLEAN)
```

---

## 2. Scraping Engine Architecture & Resilience

### 2.1 Scraping Targets
- **Primary Source (Diecast)**: Hot Wheels Fandom Wiki (`https://hotwheels.fandom.com/wiki/Hot_Wheels`)
  - Casting archives, series releases, variation tables.
  - Attribution: CC-BY-SA license link preserved in extraction metadata.
- **Secondary Sources (TCG)**:
  - Pokémon TCG API / Bulbapedia.
  - Scryfall API (Magic: The Gathering).
  - TCGPlayer API (Market index references).

### 2.2 Anti-Bot & Outage Resilience
- Headless browser pool (Puppeteer/Playwright) with user-agent rotation and randomized delays (min 2000ms between requests).
- Local schema caching and offline snapshotting to isolate catalog ingestion from external source downtime.
- Manual JSON/CSV bulk import in the Admin Console as a fallback when scrapers are challenged.

---

## 3. Asynchronous AI Condition & Anti-Fraud Pipeline

### 3.1 Step-by-Step Processing Pipeline
```
[User Submits Listing Photos]
         │
         ▼
[Stage 1: Optimistic Draft Creation]
   ├── Listing inserted with status = 'Pending AI Verification'
   └── Background Celery/Redis worker job enqueued (SLA < 60s)
         │
         ▼
[Stage 2: Plagiarism & Stolen Photo Detection]
   ├── Compute perceptual hash (pHash) against internal scraped archive & active listings
   ├── Compute visual vector embedding (CLIP ViT-B/32) for cosine similarity
   └── Inspect EXIF metadata (camera model, creation timestamp, software signatures)
         │
         ├── Similarity > 0.92 or Duplicate pHash? ──► YES ──► Route to Admin Fraud Queue
         ▼ NO                                                  (Listing hidden)
[Stage 3: AI Counterfeit / Authenticity Analysis]
   ├── Inspect card art typography, blister seal texture, foil reflectance
   └── Fake probability > 70%? ──► YES ──► Route to Admin Fraud Queue
         ▼ NO
[Stage 4: Condition Grading & Defect Estimation]
   ├── Evaluate blister integrity, soft corners, creases, scratches, chips
   └── Compute advisory condition score (1-10 scale)
         ▼
[Stage 5: Automated Publication]
   └── Listing transitions to 'Active' with public AI Advisory Badge
```

---

## 4. P2P Marketplace Reservation State Machine

```
[Active] ──► (Buyer commits) ──► [Reserved] ──► (Seller confirms & dispatches) ──► [InTransit]
   │                                   │                                                │
   │                                   ├──► (7-Day Timeout / Unresponsive)             ├──► (Courier Return)
   │                                   │    └──► [CancelledBySystem]                    │    └──► [CourierReturned]
   │                                   │                                                │
   │                                   └──► (Mutual Cancellation)                       └──► (Delivered & Paid)
   │                                        └──► [CancelledByMutual]                         └──► [Completed / Sold]
   │                                                                                                   │
   ▼                                                                                                   ▼
[Paused] (Subscription lapsed)                                                                  [Feedback Unlocked]
```

### 4.1 Transition Safeguards
1. **Atomic Guard**:
   `UPDATE listings SET status = 'Reserved' WHERE id = :id AND status = 'Active';`
2. **Phased Contact Disclosure**: Full shipping details withheld until seller confirms order.
3. **Anti-Zombie Guard**: 7-day TTL auto-cancelling unfulfilled reservations back to `Active`.
4. **14-Day Delivery Auto-Completion Guard**: Orders in `InTransit` for 14 days without dispute automatically complete.

