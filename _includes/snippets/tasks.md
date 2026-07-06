# Tasks: Hotel Trip Planner

**Feature**: [spec.md](./spec.md) | **Plan**: [plan.md](./plan.md) | **Date**: 2026-07-03
**Inputs**: plan.md, research.md, data-model.md, contracts/api.md, quickstart.md

## Conventions

- **[P]** = can run in parallel (different files, no dependency on an unfinished task).
- Each task lists concrete file paths. Tests precede the implementation they cover.
- Tasks are grouped so each **user story** is independently completable and testable.

## Assumptions carried from the checklist (defaults — override if wrong)

Resolutions for the open items in [checklists/requirements.md](./checklists/requirements.md),
applied so tasks are unambiguous:

- **Hotel identity across sources (CHK008)**: match on name + coordinate proximity +
  address; below a confidence threshold, the affected link (Google/vendor/Reddit) is marked
  **unavailable** rather than risk a wrong link (consistent with FR-008/FR-027).
- **Availability (CHK007)**: a hotel is *available* iff Expedia returns a price for the
  dates + occupancy; otherwise rejected `no_availability`.
- **Currency (CHK036)**: v1 requires the price-range currency to match the Expedia listing
  currency — **no FX conversion** performed. Mismatch → surfaced as an error/notice.
- **"Up to 10" (CHK014)**: 10 = **total hotels checked** (selected + rejected combined).
- **>10 in radius (CHK032)**: keep the **nearest 10** by distance.
- **Vendor site (CHK009)**: taken from the Expedia/Google listing's official-site field
  when present; otherwise unavailable.
- **Distance (CHK016)**: straight-line **haversine** from the selected place.
- **Timezone (CHK037)**: "not in the past" uses the **local machine timezone**.
- **Max stay (CHK010)**: capped at **30 nights**.

---

## Phase 1: Setup

- [ ] T001 Create repo layout: `backend/` and `frontend/` per plan.md structure.
- [ ] T002 [P] Initialize Go module in `backend/go.mod` (Go 1.22+); add deps:
      `modernc.org/sqlite`, `github.com/chromedp/chromedp`, `golang.org/x/sync/errgroup`.
- [ ] T003 [P] Scaffold frontend in `frontend/` (Vite + React + TypeScript); add deps:
      `leaflet`, `react-leaflet`; configure Chrome as target browser.
- [ ] T004 [P] Tooling: `gofmt`/`go vet` config for backend; ESLint + Prettier for frontend.
- [ ] T005 [P] Backend config loader in `backend/internal/config` (port, db path, default
      `radius_km=2.0`, per-source rate cap ~10/min, geocoder/map provider).

## Phase 2: Foundational (blocks all user stories)

- [ ] T006 Domain types in `backend/internal/model/` — `SearchRun`, `Hotel`, `PriceQuote`,
      `SourceLinks`, `ResultRow` (per data-model.md).
- [ ] T007 SQLite migrations in `backend/migrations/` — `search_run`, `hotel`,
      `price_quote`, `source_links` tables + validation-relevant constraints (data-model.md).
- [ ] T008 Store package `backend/internal/store/` — open DB (`modernc.org/sqlite`), run
      migrations on start, insert SearchRun/Hotel/PriceQuote/SourceLinks (audit; never
      serves prices — FR-019/FR-025).
- [ ] T009 Shared fetch package `backend/internal/fetch/` — HTTP client, **per-source token
      bucket (~10 req/min, FR-028)**, retry-with-backoff, per-fetch `context` timeout
      (FR-026). Include unit tests for the token bucket rate (SC-014).
- [ ] T010 HTTP server `backend/internal/httpapi/` — bind **127.0.0.1 only** (FR-021,
      refuse non-loopback), Go 1.22 method routing, JSON error model (contracts/api.md),
      request validation helpers + **output escaping of external content** (FR-023).
- [ ] T011 [P] `GET /api/config` handler returning effective config (contracts/api.md).
- [ ] T012 SSE progress channel scaffolding in `httpapi` for long-running search (FR-024).
- [ ] T013 [P] Frontend API client `frontend/src/api/` — typed to contracts/api.md
      (search request/response, SSE parsing, config, history).

**Checkpoint**: server boots on loopback, DB migrates, config + rate limiter in place.

---

## Phase 3: User Story 1 — Find in-budget hotels for a location + dates (P1) 🎯 MVP

**Goal**: Enter a selected location, per-night price range, and dates → get a list of
in-budget, available hotels each with a sourced Expedia price.
**Independent test**: quickstart.md steps 1–3.

### Tests (write first)

- [ ] T014 [P] Contract test for `POST /api/search` happy path + validation errors
      (`invalid_dates`, `invalid_price_range`, `location_not_selected`) in
      `backend/internal/httpapi/search_test.go`.
- [ ] T015 [P] Unit tests for the classifier (Expedia governs; over_budget / under_range /
      no_availability / no_expedia_price) in `backend/internal/search/classify_test.go`.
- [ ] T016 [P] Expedia adapter test with recorded fixtures (discovery + per-night price +
      availability) in `backend/internal/sources/expedia_test.go`.
- [ ] T017 [P] Haversine radius + nearest-10 test in `backend/internal/search/geo_test.go`.

### Implementation

- [ ] T018 Expedia adapter `backend/internal/sources/expedia.go` — chromedp render of area
      + date search; extract candidate hotels, coordinates, per-night price (price of
      record), availability; return `unavailable` on block/timeout (FR-027).
- [ ] T019 Geo utilities `backend/internal/search/geo.go` — haversine distance, filter to
      `radius_km`, keep nearest 10.
- [ ] T020 Search orchestrator `backend/internal/search/orchestrator.go` — discovery →
      radius/nearest-10 → classify by per-night budget (currency must match; occupancy
      fixed 2 adults) → build selected/rejected with reasons → **sort the selected list by
      Expedia per-night price ascending (FR-017)**; run under 60s target but **rate cap
      wins** (FR-024/FR-028).
- [ ] T021 Persist the run: write SearchRun + Hotels + Expedia PriceQuotes to store (T008).
- [ ] T022 `POST /api/search` handler — validate, invoke orchestrator, stream progress via
      SSE, return `SearchResult` with `price_of_record` references (contracts/api.md).
- [ ] T023 [P] Frontend `SearchForm` `frontend/src/components/SearchForm.tsx` — price range,
      currency, dates, and **search radius in km (default 2)** with client validation
      (end>start, not past, ≤30 nights, radius>0). Radius change updates the map circle
      (T025).
- [ ] T024 [P] Frontend `LocationAutocomplete` `frontend/src/components/LocationAutocomplete.tsx`
      — type-ahead via geocoder, explicit selection returns coords (FR-001a/b).
- [ ] T025 [P] Frontend `MapView` `frontend/src/components/MapView.tsx` — Leaflet map,
      recenters on selected place, draws radius circle (FR-001c).
- [ ] T026 Frontend `ResultsTable` (selected) `frontend/src/components/ResultsTable.tsx` —
      render selected hotels with Expedia price + source + retrieval date + currency (FR-010).
- [ ] T027 Wire `App.tsx` flow: select location → submit → progress → selected table.

**Checkpoint**: US1 delivers a usable MVP end to end in Chrome.

---

## Phase 4: User Story 2 — See why hotels were rejected (P2)

**Goal**: Show a second table of checked-but-rejected hotels, each with a reason.
**Independent test**: quickstart.md step 4 (impossible price range → rejected/over_budget).

- [ ] T028 [P] Test: response includes distinct `selected` and `rejected` arrays; every
      rejected row has a `rejection_reason` (extend `search_test.go`).
- [ ] T029 Ensure orchestrator emits all rejection reasons incl. `no_expedia_price`
      (extend T020) with sourced observed price where one exists.
- [ ] T030 Frontend: render the **Rejected** table with reason column, reusing
      `ResultsTable` (`frontend/src/components/ResultsTable.tsx`), shown alongside selected.

**Checkpoint**: both lists render; rejection transparency complete.

---

## Phase 5: User Story 3 — Cross-reference links incl. Reddit (P2)

**Goal**: Each row links to Expedia, Google Hotels (reviews/photos), vendor site (if any),
and a specific Reddit thread (if any).
**Independent test**: for a returned hotel, each present link opens the correct destination;
missing ones read "unavailable".

### Tests (write first)

- [ ] T031 [P] Reddit adapter test: specific-thread match above threshold → thread URL;
      below → `unavailable` (never search/subreddit root, FR-008) in
      `backend/internal/sources/reddit_test.go`.
- [ ] T032 [P] Hotel-identity matcher test (name + coords + address; low confidence → link
      unavailable) in `backend/internal/search/match_test.go`.

### Implementation

- [ ] T033 [P] Google Hotels link builder `backend/internal/sources/googlehotels.go` —
      construct reviews/photos deep link; price never parsed for budget (FR-007, SC-009).
- [ ] T034 [P] Vendor resolver `backend/internal/sources/vendor.go` — official-site field
      from listing when present, else unavailable (FR-009).
- [ ] T035 Reddit adapter `backend/internal/sources/reddit.go` — search hotel name + city,
      rank by relevance, return specific thread or `unavailable` (FR-008).
- [ ] T036 Hotel-identity matcher `backend/internal/search/match.go` — confidence scoring;
      gate each enrichment link on confidence.
- [ ] T037 Extend orchestrator to enrich each hotel with Google/vendor/Reddit links under
      the per-source rate cap + bounded concurrency (T009), persist `SourceLinks` (T008).
- [ ] T038 Frontend: add Expedia / Google / vendor / Reddit link cells to `ResultsTable`,
      rendering "unavailable" (not a fabricated link) when absent (FR-027).

**Checkpoint**: all four link types present per row across both tables.

---

## Phase 6: Polish & Cross-Cutting

- [ ] T039 [P] Graceful degradation: on 60s deadline (with cap in force) return `status:
      "partial"` and mark unfinished sources unavailable (SC-010); verify in a test.
- [ ] T040 [P] Progress-indicator UI consuming SSE `frontend/src/components/Progress.tsx`
      (FR-024).
- [ ] T041 [P] History endpoints `GET /api/searches` and `GET /api/searches/{id}` +
      minimal history view; served prices labeled historical, never re-presented as
      current (FR-019).
- [ ] T042 [P] Error-state UI: geocoder/map unavailable, no results, currency mismatch
      (contracts/api.md error codes).
- [ ] T043 [P] Security tests: loopback-only binding (SC-011); external content escaping
      (FR-023); SSRF — location never used as a fetch URL (FR-022).
- [ ] T044 [P] End-to-end happy-path test covering quickstart.md steps 1–4.
- [ ] T045 [P] Update `README` / verify quickstart.md instructions run clean.

---

## Dependencies

- **Setup (T001–T005)** → everything.
- **Foundational (T006–T013)** → all user stories. Within it: T006→T007→T008; T009/T010
  independent-ish; T012 before T022; T013 before frontend tasks.
- **US1 (T014–T027)** depends on Foundational. Tests T014–T017 before impl T018–T022.
  Frontend T023–T027 depend on T013 and (for live data) T022.
- **US2 (T028–T030)** depends on US1's orchestrator/classifier (T020) + results table (T026).
- **US3 (T031–T038)** depends on Foundational + US1 discovery (T018) for candidate hotels;
  T036 (matcher) gates T037.
- **Polish (T039–T045)** depends on the stories it touches.

## Parallel execution examples

- After T001: run T002, T003, T004, T005 together (different files).
- In Foundational: T011 and T013 in parallel once T010/contracts are stable.
- US1 tests T014, T015, T016, T017 in parallel (different test files).
- US1 frontend T023, T024, T025 in parallel (independent components).
- US3 adapters T033, T034 in parallel; T031/T032 tests in parallel.
- Most of Polish (T039–T045) is parallelizable.

## Implementation strategy

1. **MVP = Setup + Foundational + US1.** This alone is a working, demoable product: pick a
   location on a map, set budget + dates, see sourced in-budget hotels.
2. Add **US2** (rejection transparency) — small increment on the same orchestrator.
3. Add **US3** (Google/vendor/Reddit links + identity matching) — the enrichment layer.
4. **Polish** — partial-results, progress UI, history, security tests, E2E.

Each phase ends at a checkpoint that is independently testable, so you can stop after any
story and still have something that works.
