# Feature Specification: Hotel Trip Planner

**Feature Branch**: `001-hotel-trip-planner`
**Created**: 2026-07-03
**Status**: Draft
**Input**: "Build a trip planner web application (hotels only) with a UI for entering
preferences (location, price range, stay start/end dates). For each candidate hotel,
check Expedia, Google Hotels, and the hotel's own vendor website when available, and
find a Reddit link when available. Output a list of selected hotels and a list of
rejected hotels that were checked, with links to Expedia, Google, and Reddit per row."

---

## Constitution Alignment

This feature is bound by the [TripPlanner Constitution](../../.specify/memory/constitution.md).
Every price shown MUST carry a resolvable reference (source, URL, retrieval date,
currency) per Principle I, and stale prices MUST be flagged per Principle II. The
requirements below inherit these obligations.

---

## Clarifications

### Session 2026-07-03 (security & performance)

- Q: How will the app be deployed/accessed? → A: **Local, single-user only** — runs on
  localhost, never exposed to the public internet.
- Q: Acceptable end-to-end time for a full search? → A: **Up to 60 seconds (thorough)** —
  prioritize completeness over speed; retry slow/blocked sources; show progress.
- Q: Cache fetched prices or always fetch fresh? → A: **Always fetch fresh** — no caching;
  every search hits live sources.
- Q: How to govern outbound fetching to Expedia/Google/Reddit? → A: **Throttled + graceful
  degrade** — rate-limit outbound fetches, retry with backoff, and mark a source
  unavailable (never fabricate) if it blocks or fails.

### Session 2026-07-03 (approvals)

- FR-019 "stateless" **amended** to allow an auditable SQLite history/provenance store;
  prices are never served from storage (approved by user).
- **Legal/ToS risk** of automated retrieval from Expedia/Google Hotels/Reddit is
  **accepted by the user** for this personal, local, low-volume tool.
- Q: Scope of the ~10 req/min cap + precedence vs. the 60s budget? → A: **~10/min per
  source** (Expedia, Google, Reddit each), **cap wins** — the search may run past 60s to
  honor the cap rather than skip sources (FR-028).

### Session 2026-07-03 (analyze fixes)

- FR-011 staleness re-scoped to **historical/audit records only** (live results are never
  stale under no-cache FR-025).
- FR-013 fixed to **no FX conversion in v1** — range currency must match the Expedia
  listing currency; mismatch surfaced as an error.
- FR-017 sort (Expedia price ascending) wired into the orchestrator task (T020).
- **Search radius is a UI input in kilometers** (default 2 km) — added to FR-001, FR-016,
  and the SearchForm task (T023). "Up to 10" clarified as 10 *total checked*.
- **Vendor pricing (F5) resolved**: the vendor site is a **link only** in v1 — no price is
  fetched or stored from it. Price of record remains Expedia.

---

## User Scenarios & Testing

### User Story 1 - Find hotels within budget for a location and date range (Priority: P1)

A traveler enters a destination, a nightly-or-total price range, and check-in/check-out
dates, and receives a list of hotels that match, each backed by real, sourced prices
from Expedia, Google Hotels, and (when available) the hotel's own site.

**Why this priority:** This is the core value of the product. Without it, nothing else
matters; it is independently shippable and demonstrable.

**Independent Test:** Enter a known city and a wide price range with valid dates; verify
a non-empty list of matching hotels returns, each with at least one sourced price and a
working booking link.

**Acceptance Scenarios:**

1. **Given** a valid location, price range, and start/end dates, **When** the traveler
   submits the form, **Then** the system returns a list of hotels that fall within the
   price range for those dates, each with a price and a resolvable reference.
2. **Given** a hotel considered by the system, **When** it is displayed, **Then** the row
   shows a price with its source, the retrieval date, and the currency.
3. **Given** check-out is on or before check-in, **When** the traveler submits, **Then**
   the system rejects the input and explains the date error without running a search.

---

### User Story 2 - See why hotels were rejected (Priority: P2)

The traveler wants transparency: alongside selected hotels, they see the hotels that were
checked but not selected, and the reason each was rejected (e.g., over budget, no
availability for the dates).

**Why this priority:** Trust and auditability. It builds confidence that the search was
thorough and the selection honest, but the product is still usable without it.

**Independent Test:** Run a search with a narrow price range; verify some checked hotels
appear in a "rejected" list with a stated rejection reason.

**Acceptance Scenarios:**

1. **Given** a completed search, **When** results are shown, **Then** there are two
   distinct lists: selected hotels and rejected-but-checked hotels.
2. **Given** a hotel priced above the traveler's maximum, **When** results are shown,
   **Then** it appears in the rejected list with reason "over budget" and its observed
   price and reference.
3. **Given** a hotel with no availability for the requested dates, **When** results are
   shown, **Then** it appears in the rejected list with reason "no availability".

---

### User Story 3 - Cross-reference each hotel across sources including Reddit (Priority: P2)

For every hotel in either list, the traveler can open the Expedia listing, the Google
Hotels listing, the hotel's own site (when available), and a Reddit link (when available)
to research the hotel before booking.

**Why this priority:** The Reddit link is explicitly called out as extremely important for
the traveler's decision-making, and multi-source links are a stated core output.

**Independent Test:** For a returned hotel, confirm each present link opens the correct
external destination for that specific hotel.

**Acceptance Scenarios:**

1. **Given** any hotel in the selected or rejected list, **When** it is displayed,
   **Then** the row includes an Expedia link and a Google Hotels link for that hotel.
2. **Given** a hotel with a discoverable Reddit reference, **When** it is displayed,
   **Then** the row includes a Reddit link for that hotel.
3. **Given** a hotel with no discoverable Reddit reference, **When** it is displayed,
   **Then** the Reddit link is clearly marked as unavailable rather than pointing to an
   unrelated or fabricated destination.
4. **Given** a hotel with its own vendor website, **When** it is displayed, **Then** the
   row includes a link to that vendor site.

---

### Edge Cases

- Location matches multiple places (e.g., "Springfield") — resolved by autocomplete: the
  traveler selects one specific place from the suggestions, which anchors the search
  (FR-028–030). The system does not guess among ambiguous matches.
- Autocomplete/geocoding or map service is unreachable — the traveler cannot select a
  precise location; the system MUST report this rather than run a search against an
  unresolved location.
- A source (Expedia, Google Hotels, or vendor site) is unreachable or returns no data for
  a hotel — the hotel is still shown with the sources that succeeded, and missing sources
  are marked unavailable (not fabricated), per Constitution Principle III.
- Different sources report different prices for the same hotel — all observed prices are
  shown with their individual references, but the **Expedia** per-night price governs the
  in-budget/over-budget decision (Google Hotels prices are treated as unreliable and are
  never used for the budget decision).
- Expedia has no price for a hotel — the hotel cannot be classified against budget on the
  price of record; it is placed in the rejected list with reason "no Expedia price".
- Price is available but stale (> 7 days) — flagged `STALE` and re-verified before final
  presentation, per Constitution Principle II.
- No hotels match the criteria at all — the selected list is empty and the traveler is
  told so clearly, with the rejected list still shown if any were checked.
- Dates in the past, or a stay longer than a source supports quoting.
- Currency mismatch between price range input and source prices — see FR-013.

## Requirements

### Functional Requirements

- **FR-001**: The system MUST provide a web UI where the traveler inputs a location, a
  price range (minimum and maximum), a stay start date, a stay end date, and a search
  radius in kilometers (default 2 km).
- **FR-001a**: The location field MUST use a location autocomplete/geocoding service that
  suggests matching places as the traveler types.
- **FR-001b**: The traveler MUST explicitly select one place from the autocomplete
  suggestions; that selected place (with its geographic coordinates) anchors the search
  and its radius. A free-typed location that has not been selected MUST NOT be searched.
- **FR-001c**: The system MUST display the selected location on a map, and SHOULD show the
  configured search radius (default 2 km) around it so the traveler can see the search
  area.
- **FR-002**: The system MUST validate that the end date is after the start date and that
  dates are not in the past, rejecting invalid input with a clear message.
- **FR-003**: The system MUST search for hotels matching the location and dates and, for
  each candidate hotel, retrieve pricing/listing data via the web fetch mechanism from
  Expedia (the price of record) and Google Hotels (for customer reviews and photos, not
  price). It MUST resolve the hotel's own vendor website as a **link only** when available;
  no price is fetched or stored from the vendor site in v1.
- **FR-004**: The system MUST classify each checked hotel as **selected** (within the
  per-night price range and available for the dates) or **rejected** (checked but not
  selected), using the **Expedia** price as the governing price for the budget decision.
- **FR-005**: The system MUST present two distinct output lists — selected hotels and
  rejected-but-checked hotels.
- **FR-006**: For each rejected hotel, the system MUST state a rejection reason (e.g.,
  over budget, under-range, no availability).
- **FR-007**: For each hotel in either list, the system MUST display an Expedia listing
  link and a Google Hotels listing link for that specific hotel. The Google Hotels link
  is provided so the traveler can view customer reviews and photos; Google Hotels prices
  MUST NOT be used for the budget decision (they are considered unreliable).
- **FR-008**: For each hotel, the system MUST display a Reddit link whenever one is
  available for that hotel, pointing to a **specific discussion thread** about that hotel,
  and MUST clearly mark it unavailable when none is found — never linking to a subreddit
  root, a bare search, or a fabricated/unrelated destination.
- **FR-009**: For each hotel that has its own vendor website, the system MUST display a
  link to that vendor site.
- **FR-010**: The system MUST display, for each hotel, the Expedia price of record
  accompanied by a resolvable reference (source name, direct URL, retrieval date,
  currency) per Constitution Principle I. Any additional source prices shown MUST also
  carry their own reference.
- **FR-011**: Because every search fetches fresh (FR-025), **live results are never
  stale**. Staleness therefore applies only to *historical* audit records (FR-019): when a
  stored quote older than 7 days is shown from history, it MUST be labeled `STALE` /
  historical and MUST NOT be presented as a current price, per Constitution Principle II.
- **FR-012**: The system MUST NOT fabricate or silently estimate prices; any non-quoted
  value MUST be labeled `ESTIMATE` with its basis, per Constitution Principle III.
- **FR-013**: The price range is interpreted as a **per-night** range with an explicit
  currency. In v1 the range currency MUST match the Expedia listing currency and the
  comparison is made directly in that currency — **no FX conversion is performed**. A
  currency mismatch MUST be surfaced to the traveler (see error model) rather than silently
  converted.
- **FR-014**: The scope is hotels only; the system MUST NOT offer flights, cars, or other
  travel products.
- **FR-015**: Occupancy defaults to **2 adults (1 room)**. This is not a traveler input
  in this version; prices are retrieved for that occupancy.
- **FR-016**: The system MUST return up to **10 results total** (selected + rejected
  combined) located within a **search radius** of the location. The radius is a traveler
  input entered in **kilometers**, defaulting to **2 km**.
- **FR-017**: The system MUST sort the selected list by Expedia per-night price ascending
  (see Assumptions).
- **FR-018**: The Reddit link MUST point to a specific discussion thread about the hotel
  (see FR-008).
- **FR-019**: The system MUST persist an auditable history of searches and the price
  quotes fetched (source, URL, retrieved-at, currency); it MUST NOT serve prices from
  storage — every search fetches fresh (see FR-025). No user accounts are required (local
  single-user). *(Amended 2026-07-03: was "stateless"; revised to permit the SQLite
  provenance/history store introduced in the plan.)*
- **FR-020**: The system MUST retrieve source data using the **web fetch** mechanism.

### Security Requirements

- **FR-021**: The application MUST run as a local, single-user tool bound to the local
  machine (e.g., localhost) and MUST NOT be served on a public network interface. No
  user authentication is required in this version because it is not publicly reachable.
- **FR-022**: The system MUST only fetch from the intended source families (Expedia,
  Google Hotels, Reddit, vendor sites it has itself resolved for a candidate hotel, and
  the location-autocomplete/geocoding and map services). It MUST NOT fetch arbitrary
  user-supplied URLs; the location input is treated as search text, never as a fetch
  target, to avoid server-side request forgery.
- **FR-023**: The system MUST treat all external source content as untrusted input:
  fetched data MUST be sanitized/escaped before display so that content from an external
  page cannot inject active markup or scripts into the results UI.

### Performance Requirements

- **FR-024**: A full search targets **≤60 seconds** end to end and MUST show a progress
  indication while running rather than appearing unresponsive. When the outbound rate cap
  (FR-028) requires more time, the cap takes precedence and the search MAY run beyond 60s
  rather than skipping sources.
- **FR-025**: The system MUST NOT cache prices; every search fetches fresh data from live
  sources. (This inherently satisfies the freshness obligation in Constitution Principle
  II, so no result is ever stale.)
- **FR-026**: The system MUST govern outbound fetching politely: throttle requests per the
  rate cap (FR-028), retry transient failures with backoff, and apply a per-fetch timeout
  so a single slow or blocked source cannot hang the search.
- **FR-027**: When a source blocks, times out, or otherwise fails for a hotel, the system
  MUST mark that source unavailable for the hotel and continue, never fabricating a price
  or link to fill the gap (per Constitution Principle III).
- **FR-028**: Outbound requests MUST be rate-limited to **≈10 requests per minute per
  source** — Expedia, Google Hotels, and Reddit are each governed independently by a
  per-source token bucket. This cap takes precedence over the 60-second target (FR-024): if
  staying within the cap requires more time, the search runs longer rather than exceeding
  the cap or dropping sources. (Vendor sites and the geocoder/map service are fetched
  politely but are not part of this per-source cap.)

### Key Entities

- **Search Request**: The traveler's inputs — a **selected place** (name plus geographic
  coordinates, chosen from autocomplete), per-night price range (min, max, currency),
  start date, end date, and search radius (default 2 km). Occupancy is fixed at 2 adults /
  1 room in this version.
- **Hotel**: A candidate property — name, location, and its own vendor website URL when
  available.
- **Price Quote**: An observed price for a hotel — amount, currency, source (Expedia /
  Google Hotels / vendor), direct URL, retrieval date, freshness state, and conditions.
- **Source Links**: Per hotel — Expedia link, Google Hotels link, vendor-site link (if
  any), and Reddit link (if any), each either present or marked unavailable.
- **Result Row**: A hotel's presentation in a list — its classification (selected /
  rejected), rejection reason if applicable, its governing price with reference, and its
  source links.

## Success Criteria

### Measurable Outcomes

- **SC-001**: A traveler can go from an empty form to a results page with selected and
  rejected lists in a single submission.
- **SC-002**: 100% of prices shown carry a resolvable reference (source, URL, retrieval
  date, currency); zero unsourced prices appear in any final result.
- **SC-003**: 100% of hotels in both lists display an Expedia link and a Google Hotels
  link that resolve to that specific hotel.
- **SC-004**: For hotels where a Reddit reference exists, a Reddit link is present; where
  none exists, the row shows "unavailable" and never a fabricated or mismatched link.
- **SC-005**: Every rejected hotel shows a rejection reason.
- **SC-006**: No price presented in a final result is older than 7 days without being
  flagged and re-verified.
- **SC-007**: The application never surfaces non-hotel travel products.
- **SC-008**: A search returns at most 10 hotels, all within the configured radius
  (default 2 km) of the requested location.
- **SC-009**: The budget decision for every hotel is based on its Expedia per-night price;
  no hotel is selected or rejected on the basis of a Google Hotels price.
- **SC-010**: A full search completes within 60 seconds, or returns partial results with
  the timed-out sources clearly marked unavailable.
- **SC-011**: The application is not reachable from outside the local machine.
- **SC-012**: No cached price is ever served; every displayed price was fetched during the
  current search.
- **SC-013**: A search runs only against a location the traveler explicitly selected from
  autocomplete, and that location is shown on a map before results are produced.
- **SC-014**: During any search, outbound requests to Expedia, Google Hotels, and Reddit
  never exceed ~10 per minute per source.

## Assumptions

- The traveler is planning a single continuous hotel stay defined by one start and one end
  date.
- "Selected vs. rejected" is driven by the Expedia per-night price range and date
  availability; any additional selection criteria are out of scope for this version.
- Data is retrieved via the web fetch mechanism (FR-020); results depend on what each
  source publicly returns for a given hotel, occupancy (2 adults), and date range.
- Default sort of the selected list is Expedia per-night price ascending (FR-017), chosen
  because price is the traveler's primary stated concern; this can be revisited without
  changing scope.
- Searches are persisted for audit/history (FR-019, amended); no user accounts in this
  version. Prices are never served from storage — always fetched fresh (FR-025).
