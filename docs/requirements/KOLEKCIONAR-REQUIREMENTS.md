# Kolekcionar.app — System & Product Requirements Specification

**Document Version:** 2.1  
**Status:** Hardened & Gap-Remediated Specification  
**Target Market:** Serbia (Initial Phase) with architectural readiness for the Western Balkans  
**Primary Business Domain:** Modern Collectibles (Diecast, TCG, Figures, Memorabilia)  
**Platform Tagline:** *"Earn and collect at the same time."*

---

## 1. Executive Summary & Core Objectives

**Kolekcionar.app** is a specialized web platform designed for collectors of modern collectibles. It combines:
1. An authoritative **Master Data Catalog (MDM)** powered by intelligent web scraping, schema validation, and LLM-assisted curation.
2. A **Personal Collection & Portfolio Management Tool** (free for collectors) with lot-level acquisition, currency conversion, valuation, and margin tracking.
3. A **Curated Marketplace** enabling collectors to buy and sell items directly, monetized via a monthly seller membership.
4. An **AI-Powered Authentication & Condition Advisory Engine** protecting buyers from counterfeits and stolen internet photos via asynchronous computer vision and embedding analysis.
5. A **Community Events Notice Board** supporting verified local in-person collector meetups (*Berze*).

---

## 2. Localization, Language & Search Architecture

### 2.1 Multilingual User Interface
* The web platform UI, navigation, category taxonomy, system notifications, help guides, and legal terms are localized into three language modes:
  * **Serbian (Latin script)** — `sr-Latn`
  * **Serbian (Cyrillic script)** — `sr-Cyrl`
  * **English** — `en`
* Language selection persists in user preferences and session storage.

### 2.2 Collectible Item Nomenclature (Strict English Master Data)
* Official collectible names, brand titles, casting names, card set names, variants, and manufacturer attribute keys/values are maintained **strictly in English** in the master catalog to match global market indexing.
* Example: An item is cataloged as `"Hot Wheels Boulevard - Nissan Skyline GT-R (R34) - #71"`, never translated to Serbian in the database.

### 2.3 Cross-Script, Diacritic-Insensitive & Phonetic Search
* The search engine implements a multi-stage text normalization and matching pipeline:
  1. **Unicode Normalization & Diacritic Stripping**: Input text undergoes Unicode NFD/NFKD decomposition; Serbian Latin diacritics (`č`, `ć`, `ž`, `š`, `đ`) are indexed alongside ASCII equivalents (`c`, `c`, `z`, `s`, `dj`).
  2. **Serbian Digraph Mapping**: Special handling for digraphs (`dž`, `lj`, `nj`) ensuring parity across script representations.
  3. **Bi-directional Script Transliteration**: Cyrillic queries (e.g., `Хот Вилс`, `Нисан`) automatically transliterate to Latin script and match English catalog records (`Hot Wheels`, `Nissan`).
  4. **Phonetic & Fuzzy Matching**: Common local phonetic aliases (e.g., `Betmen` $\leftrightarrow$ `Batman`, `Pokemoni` $\leftrightarrow$ `Pokemon`) resolve to canonical master entities via Levenshtein distance and double-metaphone indexing.

---

## 3. Product Phasing, Commercial Model & Legal Compliance

### 3.1 Phase 1 — Platform Owner Showcase & Catalog Bootstrap
* The platform launches with the platform owner (Nenad / Admin) selling personal curated inventory.
* Integrated domestic card processing and automated courier shipping (Post Express API) are active **only for the platform owner's direct inventory**.
* The core MDM catalog, web scrapers, and free personal collection modules are operational for registered users.

### 3.2 Phase 2 — Peer-to-Peer (P2P) Collector Marketplace
* The platform opens to multi-seller P2P commerce.
* To comply with Serbian commerce regulations (*Zakon o trgovini*, avoiding unlicensed escrow or direct percentage commission taking), the marketplace operates on a **Classified Reservation & Direct Settlement Model**:
  * Sellers pay a recurring **Monthly Membership Fee** to list items.
  * When a buyer confirms a purchase commitment, the listing status changes to **Reserved**.
  * Verified buyer and seller contact details are securely exchanged under controlled disclosure rules.
  * Payment and logistics are executed directly between the parties (typically Serbian Post Express *pouzećem* / Cash on Delivery, domestic bank transfer, or in-person meetup).

### 3.3 Monetization, Payment Processing & Serbian Fiscalization
* Seller subscriptions are billed monthly in Serbian Dinars (RSD).
* Permitted payment gateways:
  * **Domestic Card Processing**: Domestic merchant acquirers (ChipCard, AllSecur, Banca Intesa, or UniCredit e-commerce).
  * **NBS IPS QR Code**: National Bank of Serbia Instant Payment System (IPS Skeniraj / IPS Pokaži) for mobile banking transfers.
  * International payment gateways (Stripe, PayPal) are excluded for Phase 1 and 2.
* **Serbian e-Fiscalization Compliance (*Zakon o fiskalizaciji*)**:
  * For all domestic subscription payments and Phase 1 retail transactions, the system integrates with a certified Serbian e-fiscalization software service (ESIR / SUF / PFR).
  * Electronic fiscal receipts containing a verifiable Tax Authority QR code are automatically generated, emailed to the paying user, and archived in audit logs.
* **Subscription Lifecycle & In-Flight Order Protection**:
  * When a seller's subscription lapses or fails to renew, all active public listings are immediately transitioned to **Paused** and hidden from the public marketplace.
  * **In-Flight Order Grace Period**: Any listings currently in `Reserved`, `InTransit`, or `Disputed` status retain full functionality. Sellers retain complete access to order management, tracking details, and buyer messaging until in-flight transactions reach terminal completion.
  * Private collection records, inventory data, lots, wishlists, and user reputation remain fully intact.
  * Unsubscribed sellers cannot publish new listings or unpause existing ones. If a reserved item is cancelled while the seller is unsubscribed, it reverts to `Paused` rather than `Active`.
  * Upon subscription renewal, paused listings can be unpaused individually or in bulk with a single click.

---

## 4. Master Data Management (MDM) & Taxonomy Architecture

### 4.1 Collectibles Domain Scope
* Modern diecast cars (Hot Wheels, Matchbox, Majorette, GreenLight, Mini GT, Kaido House, Inno64, etc.)
* Trading Card Games (TCG) (Pokémon, Magic: The Gathering, Yu-Gi-Oh!, One Piece, Sports Cards)
* Figures (Funko POP!, Nendoroid, Figma, standard action figures, statues)
* Extensible taxonomy designed to support retro video games, comic books, and coins in future phases.

### 4.2 3-Tier Item Architecture
The catalog enforces strict separation between master blueprints, market variants, and individual physical copies:

```
Tier 1: Master Item / Casting / Base Card
   │   └── Represents the abstract design (e.g., "Nissan Skyline GT-R (R34)" or "Charizard #4")
   ▼
Tier 2: Release / Variant
   │   └── Specific commercial release (e.g., "2020 Fast & Furious Premium - Grey - Real Riders" or "1999 Base Set 1st Edition Shadowless")
   ▼
Tier 3: User Lot Instance
       └── Physical copy owned by a collector (Condition, Boxed/Loose, User Photos, Purchase Price)
```

### 4.3 Catalog Hierarchy & Schema
* **Category**: Diecast, TCG, Action Figures, Funko.
* **Sub-Category**: Diecast scale (`1:64`, `1:43`, `1:18`), TCG Set / Game (`Pokemon TCG`, `MTG`), Figure Line (`POP! Vinyl`).
* **Manufacturer**: Global entity shared across categories (`Mattel`, `Bandai`, `Hasbro`, `Funko`, `The Pokémon Company`).
* **Brand**: Brand identity under a manufacturer (`Hot Wheels`, `Matchbox`, `Star Wars Black Series`).
* **Sub-Brand / Line**: Specific product segment (`Hot Wheels Premium`, `Hot Wheels Mainline`, `Red Line Club`).
* **Dynamic Attribute Definitions**: Category-specific attributes stored with strict data types:
  * *Diecast*: Scale, Casting Year, Series Number, Wheel Type, Base Type (Plastic/Metal), Tampo Design, Country of Manufacture.
  * *TCG*: Set Name, Card Number, Rarity (`Common`, `Secret Rare`), Card Type, Finish (`Foil`, `Holofoil`, `Reverse Holo`), Language, Era.
  * *Funko / Figures*: Series Number, Exclusive Sticker (`Convention Exclusive`, `Chase`, `Special Edition`), Glow-in-the-Dark, Flocked.

---

## 5. Web Scraping, Data Enrichment & Ingestion Governance

### 5.1 Scraping Targets & Prioritization
* **Primary Phase 1 Source**: Hot Wheels Fandom Wiki (`https://hotwheels.fandom.com/wiki/Hot_Wheels`) for casting archives, release years, series, and variations.
* **Secondary Sources**: Pokémon TCG API / Bulbapedia / Scryfall (for MTG) / TCGPlayer API.
* **Pricing Intelligence Sources**: Scrapers extract completed and active listings from regional and international marketplaces to calculate estimated fair market values (adjusted for Serbian customs, import VAT, shipping, and local market trends).

### 5.2 Scraping Governance, Legal Adherence & Resilience
* All scrapers must strictly observe `robots.txt`, respect polite rate limits (maximum 1 request per 2 seconds per host), and prioritize documented public APIs.
* **Anti-Bot & Outage Resilience**:
  * Headless browser pool with automatic retry and user-agent rotation to handle intermittent CAPTCHA or Cloudflare challenges without compromising sandbox boundaries.
  * Local schema caching and offline snapshotting to isolate catalog ingestion from external source downtime.
  * Manual JSON and CSV catalog import capabilities in the Admin Console as a fallback for blocked scrapers.
* **Copyright & Creative Commons Compliance**:
  * Attribute data and release facts are extracted as non-copyrightable factual data.
  * Textual descriptions derived from Fandom wikis must maintain CC-BY-SA source attribution links stored in the metadata.

### 5.3 Change Request Queue & Dynamic Attribute Ingestion Gate
* Raw scraped data is never written directly to the active MDM database.
* Scraped records are deposited into a staging **Change Request Queue** in the Admin Console.
* **Unmapped Attribute Handling**: If a scraper extracts attribute keys or data types not defined in the category taxonomy schema, the ingestion pipeline flags the record and routes the unmapped fields to an **Unmapped Attribute Review Queue** rather than failing the batch job.
* The Admin can:
  * Review suggested new Master Items or Variants with side-by-side diffs.
  * Map unrecognized attributes to canonical schema or instantiate new attribute definitions.
  * Approve, edit, or reject change requests.
  * Merge duplicate entities identified by fuzzy matching algorithms.

### 5.4 Image Copyright & Public Display Strategy
* Scraped third-party images are strictly restricted to **Admin Internal Reference** to avoid copyright infringement.
* The public catalog thumbnail for any Master Item or Variant is sourced exclusively via:
  1. **Community-Sourced Verified Photos**: Clean photos uploaded by verified collectors that are approved by an Admin to become the official thumbnail.
  2. **Official Media Kit Photos**: Manufacturer-provided press/media release imagery with appropriate copyright attribution and fair-use notice.
  3. **Neutral Vector Placeholder**: High-quality category silhouette displayed until a verified image is promoted.

---

## 6. AI Condition Grading & Anti-Fraud Engine

### 6.1 Asynchronous Computer Vision & LLM Analysis Pipeline
To eliminate mobile submission latency and prevent HTTP timeouts, photo analysis runs asynchronously via a background processing worker with an optimistic user interface:

```
User Submits Photos for Listing
         │
         ▼
[Stage 1: Instant Optimistic Draft]
   ├── Listing created with status: "Pending AI Verification"
   └── Background processing job enqueued (Target SLA < 60 seconds)
         │
         ▼
[Stage 2: Plagiarism & Anti-Stock Photo Detection]
   ├── Compute perceptual hash (pHash) against internal scraped DB & active listings
   ├── Compute visual vector embedding (CLIP) for crop/rotation-invariant similarity
   └── Inspect EXIF metadata (camera model, creation timestamp, software signatures)
         │
         ├── Flagged as duplicate / stock? ──► YES ──► Route to Admin Fraud Queue
         │                                            (Listing stays hidden)
         ▼ NO
[Stage 3: AI Counterfeit / Authenticity Risk Analysis]
   ├── Inspect packaging card art, font typography, blister seals, foil reflection patterns
   └── Fake probability > 70% threshold? ──► YES ──► Route to Admin Fraud Queue
         │
         ▼ NO
[Stage 4: Condition Grading & Feature Estimation]
   ├── Evaluate blister integrity, soft corners, card creases, surface scratches, paint chips
   └── Compute advisory condition score (1-10 scale)
         │
         ▼
[Stage 5: Automated Publication]
   └── Listing status transitions to "Active" with public AI Advisory Badge
```

### 6.2 Multi-Vector Plagiarism Detection
* Sellers must upload genuine original photos of the physical item currently in their possession.
* The detection engine combines:
  1. **pHash Comparison**: Fast hamming-distance detection for identical images.
  2. **CLIP Vector Embeddings**: Cosine similarity check across high-dimensional visual feature space, robust against cropping, border additions, aspect ratio adjustments, and color filtering.
  3. **EXIF Heuristic Validation**: Checks for missing or synthetic EXIF data common in web-scraped thumbnails.
* Any positive plagiarism match blocks public publication and prompts the seller to upload genuine in-hand photos with a physical timestamp marker.

### 6.3 Moderation Flow & Escalation Thresholds
* **Low Risk Flow (Passed Checks)**: Listing automatically transitions to `Active`. Public view displays:
  * *Seller Declared Condition*: e.g., `"Near Mint (Carded)"`
  * *AI Condition Advisory*: e.g., `"AI Estimate: 9/10 (Minor soft bottom-left corner)"`
* **High Risk Flow (Escalation)**: When counterfeit risk exceeds 70%, visual similarity matches a known web image, or a blacklisted seller signature is detected:
  * Listing is held in `Pending Admin Review` and withheld from public search.
  * Flagged reasons are surfaced in the Admin Listing Moderation Queue with highlighted visual diffs.

---

## 7. Personal Collection & Portfolio Hub (Free Collector Hub)

### 7.1 Free Lifetime Tier
* Personal collection and portfolio management features are **always 100% free** for registered collectors with no mandatory subscription.

### 7.2 Multi-List Organization & Custom Groupings
* Collectors can organize items across multiple standard and custom lists:
  * **Owned Collection**: Current inventory held.
  * **Wishlist**: Desired items with automated notification alerts when matching items are listed on the marketplace.
  * **Trade / Sell Pile**: Items earmarked for possible exchange or sale.
  * **Custom Tags & Binders**: Arbitrary user groupings (e.g., *"1982 Red Hot Wheels"*, *"Charizard Master Set"*, *"Desk Display"*).

### 7.3 Lot-Level Purchase & Multi-Currency Profitability Tracking
* Collectors can track single items or grouped lots (bundles bought together).
* **Private Lot Attributes (Owner-Only Visible)**:
  * **Acquisition Price & Currency**: Recorded in RSD, EUR, or USD.
  * **Historical FX Pegging**: System records the official National Bank of Serbia (NBS) middle exchange rate on the acquisition date.
  * **Dynamic Unrealized Profit Margin**: Unrealized profit is dynamically calculated against current fair market value in base currency (RSD) to separate genuine asset appreciation from foreign exchange inflation.
  * **Acquisition Provenance**: Date acquired, seller/source (e.g., *"KupujemProdajem"*, *"Berza Ušće"*, *"Retail store"*, *"Trade"*).
  * **Private Notes**: Storage location, private condition notes, target selling price.
* **Public Showcase & Privacy Isolation**:
  * Collectors can generate a public shareable URL showcasing their collection.
  * Private purchase prices, acquisition sources, and profit margins are strictly filtered out of all public APIs and views.

---

## 8. Listing & Reservation Marketplace Flow (P2P)

### 8.1 Listing Requirements & Multi-Quantity Support
* Seller must hold an active seller subscription.
* Item must be linked to a known MDM catalog variant (or submitted via catalog request).
* Minimum of 2 high-resolution original photos (front and back/corners) required.
* Condition must be declared according to category-specific rubrics.
* **Quantity & Lot Splitting**:
  * Listings support single items (`quantity = 1`) or multi-item inventory (`quantity > 1`).
  * If a buyer reserves a partial quantity (`requested_qty < quantity`), the available quantity is decremented atomically, and a dedicated sub-reservation order is created. When remaining quantity reaches zero, the listing status transitions to `Reserved`.

### 8.2 Reservation State Machine & Settlement Lifecycle

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

#### State Transition Rules & Safeguards:
1. **Atomic Reservation (Concurrency Guard)**:
   * To prevent duplicate commitments, reserving an item executes an atomic database operation:
     `UPDATE listings SET status = 'Reserved' WHERE id = :id AND status = 'Active';`
   * Concurrent requests attempting to reserve the same unit receive an immediate conflict error (`HTTP 409`).
2. **Buyer Privacy & Phased Contact Disclosure**:
   * To protect buyers against data-harvesting bait listings, full physical delivery addresses and telephone numbers are withheld initially.
   * Reserving initiates a direct, end-to-end platform messaging channel and notifies the seller.
   * Full shipping details (buyer name, verified telephone, delivery address, preferred courier) are revealed to the seller **only after the seller explicitly confirms the order and commits to fulfillment**.
3. **Reservation Expiration Timeout (Anti-Zombie Guard)**:
   * A reservation carries a **7-day timeout (TTL)**.
   * If the seller does not dispatch or the parties fail to confirm the transaction within 7 days, the reservation expires automatically (`CancelledBySystem`), and the item reverts to `Active` status.
4. **Fulfillment & Dispatch**:
   * Seller dispatches the parcel (typically Post Express *pouzećem* / Cash on Delivery) and enters the courier tracking number, transitioning the state to `InTransit`.
   * If the package is refused at the door or returned by the courier, the seller marks it as `CourierReturned`; the listing reverts to `Active` after admin or mutual verification.
5. **Completion & Auto-Confirmation**:
   * Once delivered and settled, either party marks the transaction as `Completed`.
   * **14-Day Delivery Auto-Completion Guard**: If an order remains in `InTransit` for 14 days without a logged dispute or manual confirmation, the system marks the transaction as `Completed` and triggers the feedback prompt.
   * Completed listings transition to `Sold` status and feed into the historical pricing index.

---

## 9. Trust, Reputation & Anti-Abuse Feedback System

### 9.1 Feedback Rules & Unlocking Thresholds
* **Eligibility**: Only users who engaged in a confirmed transaction that reached `Completed` status are eligible to leave feedback.
* **Rating Schema**:
  * **Positive (`Pozitivna`)**
  * **Neutral (`Neutralna`)**
  * **Negative (`Negativna`)**
  * Accompanying text review (minimum 10 characters).
* **Anti-Extortion Guard**:
  * If a reservation is cancelled prior to dispatch due to non-payment or mutual agreement, feedback is disabled.
  * Buyers cannot submit negative ratings for unfulfilled transactions where they failed to complete payment or refused delivery.

### 9.2 Anti-Abuse, Gaming Prevention & Account Integrity
* **One Review Per Order**: Multiple items purchased together in a single reservation session aggregate into a single feedback event.
* **Mandatory Phone Verification**:
  * Users must verify a valid Serbian mobile phone number (`+381`) via SMS OTP before they can reserve items, create listings, or leave reviews.
  * Virtual VOIP numbers are blocked via carrier prefix validation.
* **Phone Number Modification Cooling Period**:
  * Updating a verified phone number triggers an automated **48-hour security cooling period**. During this window, the account is restricted from creating new listings or reserving items.
* **Feedback Dispute & Arbitration**:
  * Users can contest retaliatory or defamatory feedback via the **Feedback Arbiter** in the Admin Console.
  * Admins have full authority to inspect platform messaging transcripts, courier tracking logs, and redact bad-faith ratings.

---

## 10. Community Events Notice Board (*Berze*)

### 10.1 Event Directory & Submission Workflow
* A dedicated section listing upcoming physical collector fairs and meetups in Serbia:
  * Berza modela automobila (TC Ušće, Beograd)
  * Berza stripova i figura (Dom Omladine, Beograd)
  * Regional conventions (Novi Sad, Niš, Kragujevac).
* **Moderated Submission Lifecycle**:
  * Registered users and organizers can submit proposed community meetups.
  * Submissions enter the **Event Moderation Queue** and require Admin verification before public listing.
* **Anti-Spoofing RSVP System**:
  * RSVP actions ("Dolazim / I'm attending") are restricted to authenticated users with verified phone numbers.
  * Rate limiting prevents automated bot scripts from inflating attendee counts.
* Event detail view displays: Title, venue address, interactive map coordinates, date & hours, organizer credentials, entrance fees, and verified RSVP count.

### 10.2 Platform Liability Disclaimer
* Every event detail page displays a prominent statutory disclaimer:
  > *"Kolekcionar.app is an informational event directory and is not the organizer or guarantor of physical gatherings. Kolekcionar.app assumes no legal or financial liability for interactions, transactions, or physical trades conducted off-platform at in-person venues."*

---

## 11. Admin Console & Back-Office Workflows

The Admin Console (`/admin`) provides centralized back-office tooling aligned with core domain modules:

| Admin Module | Cross-Reference | Operational Scope |
| :--- | :--- | :--- |
| **MDM Manager** | §4 MDM & Taxonomy | Full CRUD for Categories, Sub-categories, Manufacturers, Brands, Sub-brands, Dynamic Attributes, Master Items, and Variants. |
| **Change Request Queue** | §5.3 Ingestion Gate | Staging table for web scraper output; entity diff viewer; one-click merge into production MDM. |
| **Unmapped Attribute Queue** | §5.3 Scraping Schema | Review unrecognized scraped attribute keys; assign to existing schema or create dynamic attribute definitions. |
| **Listing Moderation Queue** | §6.3 AI Moderation | Review AI-flagged listings (counterfeit risk > 70%, pHash/CLIP image match, missing EXIF, user flags). |
| **User & Subscription Admin** | §3.3 Monetization | Inspect subscription status, override billing, verify phone numbers, enforce fraud bans, audit e-fiscal invoices. |
| **Feedback Arbiter** | §9.2 Reputation System | Dispute queue for contested negative ratings; messaging log and tracking verification; review redaction. |
| **Event Directory Manager** | §10.1 Events | Review, verify, edit, and publish community meetup submissions; monitor RSVP integrity. |

---

## 12. Non-Functional Requirements (NFRs)

### 12.1 Performance & Responsiveness
* Page load time (Largest Contentful Paint - LCP) under 1.5 seconds on desktop and standard 4G mobile connections.
* Autocomplete search response latency under 150ms for cross-script queries.
* Image optimization: Uploaded user photos automatically compressed into modern formats (`WebP`/`AVIF`) with responsive breakpoints (thumbnail, card, zoom view).
* AI asynchronous verification pipeline turnaround SLA under 60 seconds per listing.

### 12.2 Security & Data Privacy
* Compliance with Serbian Data Protection Law (*Zakon o zaštiti podataka o ličnosti* - ZZPL, aligned with EU GDPR).
* Password hashing using `Argon2id` or `bcrypt` (work factor 12+).
* Private user data (phone numbers, physical delivery addresses) masked and revealed only under authorized reservation state transitions (§8.2).
* Sensitive database fields (user PII, internal lot notes) encrypted at rest using AES-256-GCM.
* Rate limiting on login attempts, SMS verification requests, search endpoints, and RSVP submissions.

### 12.3 Reliability, Auditability & Backups
* Daily automated PostgreSQL backups with point-in-time recovery (PITR) retained for 30 days.
* Multi-region redundant cloud object storage for user collection imagery.
* Immutable audit logging for e-fiscal receipt issuances, subscription billing events, and administrative moderation overrides.
