# PRD — "Lunch Picker" (Random Lunch Decider)

**Owner:** you
**Doc version:** v1.0 (MVP scope)
**Status:** Ready for build

---

## 1) Vision & Goals

**Main goal (answers your prompt):**
Create a **personal tool** (also portfolio-ready) that helps users decide what to eat for lunch **in under 10 seconds**, with the ability to **add/manage their own options** and **keep them for future use**.

**Success looks like:**

* Time-to-decision ≤ 10s (spin to selection + 1 click confirm).
* 7-day retention ≥ 40% for solo users.
* ≥ 20 saved items for power users without usability drop.
* Zero back-end dependency in MVP (offline capable). Optional cloud sync as V1.1.

**Non-goals (MVP):**

* No delivery integrations (GrabFood, Deliveroo).
* No maps or live distance/pricing.
* No multi-user collaboration (voting) in MVP.

---

## 2) Personas

1. **Solo Decider (Primary)**
   Office worker who wants a quick randomized pick from their usual places.
2. **Power Curator (Secondary)**
   Likes tagging, weighting, and excluding recent repeats.

---

## 3) Core Value Proposition

* **One-tap random pick** that respects your preferences (tags, budget, avoid repeats).
* **Zero setup friction**: works offline, saves locally; later optional cloud sync.

---

## 4) Key Features & Requirements

### MVP (Build now)

* **List management**

  * Add/edit/delete items with fields: `name` (required), `tags` (comma), `price_range` ($/$$/$$$), `notes` (optional), `weight` (1–5).
  * Persist locally via `localStorage`/IndexedDB (offline-first).
* **Randomizer**

  * "Spin/Shuffle" button → returns 1 suggestion.
  * **Cooldown rule:** avoid suggesting same item if picked in last N days (default 3), toggleable.
  * Optional **weighted random** by `weight`.
* **Filters**

  * Quick filters: price range, tags, exclude "last picked".
* **History**

  * Store last 30 picks with timestamp; allow "re-spin" and "mark eaten".
* **UX**

  * Single screen, large spin CTA, fast input form, empty-state helper ("Add your first 5 places").
* **PWA**

  * Installable icon, offline capable, load < 2s on mid-tier phone.

### V1.1 (Post-MVP)

* **Cloud sync (optional)**: Supabase/Firebase login to sync items & history.
* **Smart streaks**: rotate through cuisines before repeating.
* **Share seed list** template exports/imports (JSON).
* **A/B: spinner vs. card shuffle animation.**

---

## 5) User Stories & Acceptance Criteria

1. **Add item**

   * *Given* I open the app
   * *When* I enter "Chicken Rice", tags "Asian, Hawker", price "$", weight 3 and save
   * *Then* it appears in my list and persists after refresh.

2. **Random pick with cooldown**

   * *Given* "Chicken Rice" was picked today and cooldown=3
   * *When* I spin again today
   * *Then* the result excludes "Chicken Rice".

3. **Weighted random**

   * *Given* Item A (weight 5) and Item B (weight 1)
   * *When* I spin 100 times
   * *Then* A appears ~5x more frequently than B (± tolerance).

4. **Filters**

   * *Given* I filter by `$` and tag `noodle`
   * *When* I spin
   * *Then* only matching items are eligible.

5. **History**

   * *Given* I've picked 3 items
   * *When* I open history
   * *Then* I see timestamps; tapping one can "Pick again".

---

## 6) UX Overview (single page)

* **Header:** "Lunch Picker" + settings icon.
* **Primary CTA:** big **Spin** button centered.
* **Result Card:** item name, tags, price, notes; actions: **Pick again**, **Lock filters**, **Mark eaten**.
* **List panel:** searchable list with add/edit/delete.
* **Filters bar:** Tags (chips), Price ($/$$/$$$), **Avoid repeats [3 days]** toggle.
* **Footer:** history (last 30), import/export JSON (hidden behind "Advanced").

---

## 7) Data Model

```json
{
  "items": [
    {
      "id": "ulid|uuid",
      "name": "Chicken Rice",
      "tags": ["asian","hawker"],
      "price": "$",
      "notes": "Corner stall, fast",
      "weight": 3,
      "created_at": "2025-08-21T04:00:00Z",
      "updated_at": "2025-08-21T04:00:00Z",
      "last_picked_at": "2025-08-20T04:00:00Z"
    }
  ],
  "history": [
    { "item_id": "uuid", "picked_at": "2025-08-21T04:10:00Z" }
  ],
  "settings": {
    "cooldown_days": 3,
    "use_weighted": true,
    "filters": { "tags": [], "price": [] }
  }
}
```

---

## 8) Selection Logic (pseudocode)

```
candidates = items
  .filter(matchesSelectedTagsAndPrice)
  .filter(item => now - item.last_picked_at > cooldownDays)
if candidates empty -> fallback to all matches (ignore cooldown).

if use_weighted:
  pool = expand by 'weight'
  selection = random(pool)
else:
  selection = random(candidates)

update last_picked_at & push to history
```

---

## 9) Tech Stack

* **Frontend:** HTML/CSS/JS (or React if you prefer), PWA manifest, Service Worker.
* **Storage:** `localStorage` (MVP). **V1.1:** Supabase/Firebase for auth+sync.
* **State Mgmt:** Lightweight (vanilla or React state).
* **Build/Deploy:** Static hosting (Vercel/Netlify/GitHub Pages).

---

## 10) Non-Functional Requirements

* TTI < 2s on 3G Fast; bundle < 150KB gz (MVP).
* Works offline; no fatal errors without network.
* No personal data beyond the items list unless user opts into sync.
* Accessibility: keyboard-navigable, contrast ratio ≥ 4.5:1.

---

## 11) Analytics (optional, local first)

* Events: `add_item`, `edit_item`, `delete_item`, `spin`, `result_shown`, `filters_used`, `history_opened`.
* Store anonymously (local) in MVP; add consent banner before any remote telemetry later.

---

## 12) Risks & Mitigations

* **Empty/low variety lists →** guided empty state to add 5 starters.
* **Over-repetition fatigue →** cooldown + tag cycling (V1.1).
* **Device loss →** optional cloud sync (V1.1).
* **Analysis paralysis in filters →** minimal defaults; "I'm Feeling Lucky" ignores filters.

---

## 13) Release Plan & Timeline (suggested)

* **Week 1 (MVP):** UI, item CRUD, storage, randomizer, filters, history, PWA.
* **Week 2 (Polish):** weighted picks, cooldown, import/export JSON, animations, QA.
* **Week 3 (V1.1 opt-in):** auth + cloud sync.

---

## 14) QA Checklist (high level)

* Add/edit/delete persist across reloads.
* Spin respects filters & cooldown.
* Weighted distribution sanity test.
* PWA install & offline spin works.
* Import/export round-trips identical JSON.
* A11y: tab order, focus ring, aria labels.

---

## 15) Ready-to-Build Artifacts

### Minimal REST (for V1.1 sync)

```
GET /api/items
POST /api/items
PUT /api/items/:id
DELETE /api/items/:id
GET /api/history
POST /api/history
```

### Import/Export JSON format

* Exact as **Data Model** above.

---

If you want, I can drop a **single-file HTML app** (offline-ready) you can deploy to Vercel in minutes, and we can iterate to add cloud sync next.