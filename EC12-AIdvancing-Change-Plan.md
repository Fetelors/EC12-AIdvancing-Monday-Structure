# EC12 — AIdvancing Portal: Change Plan

## Context

The **AIdvancing Portal** is the interaction layer for the EC12 event management system. All data lives in Monday.com (10 interconnected boards). The portal provides clean, role-appropriate interactions grouped around the **Steps of Advancing** workflow — it does not store data itself; it reads from and writes back to Monday.

This change plan covers the full build-out, structured as four sequential delivery layers. Each layer must be stable before the next begins. The feature interactions and user stories from the EC11 Roadmap and EC12 Team Feedback are delivered in Layer 4.

**Sources:**
- `EC11 - Magic Make Monday & Google Sheets - Roadmap copy.txt` — interactions and user stories from the previous edition
- `EC12 - Monday - Team feedback user stories copy.txt` — EC12 team feedback and desired end states
- `plan-ec12MondayStructure.prompt.md` — full field map, board IDs, navigation structure, file manifest
- `EC12 AIdvancing Portal Design V3 copy.html` — design reference

---

## Layer 1 — Infrastructure: Monday.com Connection, Domain & Auth

**Goal:** The portal is live at a real URL, connected to the Monday.com API, with authenticated access. Nothing else is built until this layer is solid.

### Issue 1.1 — Monday.com GraphQL API integration
Set up `portal/monday-client.js` as the single API wrapper for all 10 boards.

- All 10 board IDs wired (see Board Registry in field map)
- Read: `get_board_items_page`, `get_full_board_data` patterns
- Write: `change_item_column_values`, `create_item`, `create_update`
- Rate limiting and error handling in place
- Auth token stored securely (env var / secrets manager — never in client code)

**Acceptance criteria:**
- All 10 boards can be queried from the portal with correct data returned
- Write operations update Monday records (verified in Monday UI)
- Token is not exposed in browser source

---

### Issue 1.2 — Domain setup and hosting
Deploy the portal to a stable URL accessible by the team (e.g., `advancing.electriccastle.ro` or equivalent).

- Choose hosting (static host, Vercel, or self-hosted)
- SSL certificate configured
- Environment separation: dev URL vs production URL

**Acceptance criteria:**
- Portal accessible over HTTPS at agreed URL
- Dev environment accessible at separate URL for safe testing

---

### Issue 1.3 — Authentication and role-based access
Users log in to the portal. Access is role-restricted so team members only see what's relevant to them.

- Login method: confirm approach (Monday OAuth SSO, email/password, or passkey)
- Roles: Advancing Lead, Angel, Backstage Manager, Hospitality Manager, Transport Coordinator, Admin
- Role assignment: driven by Monday Team Members board (board 18412445606), field: Team
- Session management: tokens, expiry, logout

**Open question:** Is Monday OAuth the preferred auth method, or do we maintain a separate user list? Decide before implementation.

**Acceptance criteria:**
- Users can log in and are assigned a role automatically from their Monday Team record
- Each role sees only their permitted views (see Layer 4 for view definitions)
- Admin can override role/view access

---

### Issue 1.4 — Steps of Advancing: reference integration
The portal must follow the Steps of Advancing workflow document as the canonical guide for what Advancing Leads do and in what order.

- Obtain/confirm the current Steps of Advancing document
- Map each step to: which Monday fields are read/written, which portal view or interaction handles it
- This mapping becomes the source of truth for the progress bar (Layer 4, Issue 4B.1) and phase-aware email (Layer 4, Issue 4E.1)

**Acceptance criteria:**
- Steps of Advancing document is linked in the repo (or summarised in `ADVANCING_STEPS.md`)
- Each step has a corresponding portal interaction or view identified

---

## Layer 2 — Design Layer: AIdvancing Portal Design → App Code

**Goal:** The visual design (from `EC12 AIdvancing Portal Design V3 copy.html`) is fully implemented in code. The app is navigable and looks correct, but data is mocked.

### Issue 2.1 — Design tokens and theme
Create `portal/theme-v2.css` from the V3 design file.

- Colour palette, typography, spacing, status colours
- Component tokens: buttons, chips, badges, drawers, tables
- Dark mode support if specified in design

**Acceptance criteria:**
- All colours and type styles match the V3 design file exactly
- Theme applied globally; no inline styles

---

### Issue 2.2 — Shared component library
Create `portal/components.jsx` with all reusable UI components.

Components: `TopNav`, `Blobs`, `PageFoot`, `StatusBadge`, `FieldDot`, `StatsStrip`, `SearchBar`, `Modal`, `Drawer`, `Table`, `ProgressBar`, `ActionButton`

**Acceptance criteria:**
- Each component renders correctly with mock data
- Components match V3 design file
- Drawer opens/closes smoothly from any tracker row

---

### Issue 2.3 — Navigation structure implemented
All routes navigable from the top nav with correct shells loading.

Navigation structure (shells only — data mocked at this stage):
```
tracker.artist / tracker.travel / tracker.accommodation / tracker.hospitality
magic.email / magic.routes / magic.folders
stage
reports.flights / reports.transfers / reports.by-user / reports.itineraries
databases.* (9 sub-routes)
settings
```

**Acceptance criteria:**
- Every route in the nav loads without errors
- Active state highlights correctly in nav
- Mobile breakpoints functional

---

### Issue 2.4 — Mock data layer
Create `portal/data-v2.js` with realistic mock data shaped to all 10 Monday boards.

Exposes: `window.ARTISTS`, `window.STAGES`, `window.HOTELS`, `window.FLIGHTS`, `window.TRANSFERS`, `window.TEAM`, `window.ARTIST_PARTY`, `window.HOTEL_ROOMS`, `window.DRESSING_ROOMS`, `window.ARTIST_VEHICLES`

**Acceptance criteria:**
- All views render with mock data matching Monday board field shapes
- Mock data removed/bypassed once Live API (Layer 1) is confirmed stable

---

## Layer 3 — Monday Field Configuration in Design & App Code

**Goal:** Every field shown in the portal maps 1:1 to a real Monday column. No display field is invented; no Monday field is missed that the team needs.

### Issue 3.1 — Field mapping audit
Before wiring fields in code, audit the full field map (CSV + plan doc) against the V3 design file.

- Identify any fields shown in design that don't exist in Monday → flag for addition or removal
- Identify any Monday fields needed by the team that aren't shown in design → add to relevant view
- Output: updated field map with `Design` and `Monday` columns confirmed as matched

**Acceptance criteria:**
- Field map document updated and approved by Product Owner before any field wiring begins
- Zero invented fields in the portal

---

### Issue 3.2 — Tracker views: field wiring (all 4 subviews)
Wire real Monday data into `tracker.artist`, `tracker.travel`, `tracker.accommodation`, `tracker.hospitality`.

Each column maps to a verified Monday field ID. Mirrors and formulas are read-only. Input fields write back via `change_item_column_values`.

See field map for full column-to-field-ID assignments per subview.

**Acceptance criteria:**
- All 4 tracker subviews show live Monday data
- Editable fields save back to Monday on blur/confirm
- Mirror/formula fields render correctly (read-only, no edit affordance)

---

### Issue 3.3 — Artist Drawer: 5 tabs field wiring
Wire all 5 drawer tabs (Artist, Travel, Accommodation, Hospitality, Handover) to live Monday data.

**Acceptance criteria:**
- Drawer opens from any tracker row with correct artist data loaded
- All fields per tab mapped to field map spec
- Tab 5 (Handover Notes) saves to Notes for Backstage Manager field

---

### Issue 3.4 — Databases: 9 CRUD boards field wiring
Wire all 9 database views to live Monday data with full create/read/update support.

Boards: Accommodation, Hotel Rooms, Artist Party, Flights, Transfers, Artist Vehicles, Dressing Rooms, Stages, Team Members.

**Acceptance criteria:**
- Each database view lists real records from its Monday board
- New record creation writes to Monday
- Field edits update Monday in real time
- Board relations (dropdowns linking to other boards) work correctly

---

### Issue 3.5 — Stage Programme: field wiring
Wire `stage` view (Timeline + Calendar toggle) to live Monday data from Tracker mirrors and Stage Timings board.

**Acceptance criteria:**
- Calendar grid shows all artists per stage per day, slot width ∝ Duration (m)
- 5 day tabs (16/07–20/07) working
- Soundcheck strips shown per stage row
- Click on slot opens Artist Drawer

---

### Issue 3.6 — Reports: field wiring
Wire all 4 reports (`reports.flights`, `reports.transfers`, `reports.by-user`, `reports.itineraries`) to live Monday data.

**Acceptance criteria:**
- Flights report shows all flights with artist links
- Transfers report renders as run sheet, grouped by Route Date
- By-User report filters by Advancing Lead (dropdown from Team Members)
- Itineraries generate from confirmed data only

---

## Layer 4 — Interactions & User Stories

**Goal:** All team-facing interactions are implemented as buttons, flows, and role-based views. Data writes correctly back to Monday. All user stories from EC11 Roadmap and EC12 Team Feedback are delivered.

---

### Epic 4A — Monday Board Cleanup (Pre-advancing prerequisite)

These are Monday-side changes, not portal changes. Must complete before advancing opens.

**Issue 4A.1 — Clean Artist Advancing Tracker: hide unused fields**  
Hide from board view (not delete): Actions Completed, Nights Paid by EC/Artist, Other Travel Party, Initial Rider, Ethernet/00 Req, Record Email to CC, Related Artist/Crew.  
*Acceptance:* Board shows only active workflow fields. Hidden fields documented.

**Issue 4A.2 — Deprecate Advancing Actions by Stage board**  
Archive board; remove from nav. Email templates are the replacement tracking method.

**Issue 4A.3 — Artist Party data cleanup checklist**  
Build filter view / CSV report flagging legacy EC11 data in Artist Party records. Run before advancing opens.

**Issue 4A.4 — Validate Team Members records**  
Each record: Name, Team, Role, Phone, Email, Stage connection only. Audit and remove outdated data.

**Issue 4A.5 — Validate Stage records**  
All stages: Capacity, Schedule, Stage Manager, Backstage Manager, role connections confirmed. Stage Info Status = Confirmed.

---

### Epic 4B — Advancing Tracker Interactions

**Issue 4B.1 — Progress bar per artist (Steps of Advancing)**  
Visual 7-phase progress bar per artist row derived from Steps of Advancing mapping (from Issue 1.4). Click segment to jump to relevant tab.  
*Source:* EC11 Roadmap

**Issue 4B.2 — Outstanding items badge + summary panel**  
Count of unfilled required fields shown as chip on each artist row. Click opens summary (not full drawer).  
*Source:* EC11 Roadmap — "quickly see a summary of what's outstanding"

---

### Epic 4C — KO Status Automation

**Issue 4C.1 — KO Status trigger: create Google Drive folder + standard info pack**  
When Advancing Status → Advancing KO:
1. Create Google Drive folder: `EC12 / [Stage] / [Artist Tier] / [Artist Name]`
2. Add standard information pack per stage as a shortcut inside the folder
3. Paste folder link into Artist Pack Link field on artist record in Monday

*Open question:* Which Google account / service account? Auth scope for Drive API.  
*Source:* EC11 Roadmap (updated)

---

### Epic 4D — Flight & Transfer Interactions

**Issue 4D.1 — Assign flight: luggage notes + post-assignment route prompt**  
Assign flight from existing Flights list. After saving: prompt to create a pickup route (pre-filled with Route Type = Airport→Hotel, Flight Date, Flight record). Luggage notes auto-pulled from Tracker.  
*Source:* EC11 Roadmap

**Issue 4D.2 — Generate Routes: multi-flight support + red/yellow status**  
Route builder lists all linked flights for the artist. Each route:
- Red → no passengers assigned
- Yellow → passengers assigned but no luggage/cargo  
- Green → complete  
Routes write to Transfers board (18412445611).  
*Source:* EC11 Roadmap + EC12 Team Feedback — "artists with multiple flights without manual selection of each leg"

**Issue 4D.3 — Assign passengers to routes (Artist Party members + placeholders)**  
Passengers selectable from Artist Party linked to artist. Passengers count populates Transfer Passengers field.  
*Source:* EC11 Roadmap

---

### Epic 4E — Artist Party & Placeholder Flow

**Issue 4E.1 — Bulk-create party members from names, file, or placeholder**  
When party size is set: offer to create all members at once (names list, file upload, or placeholders). Placeholders named `[Artist] Party Member N`, Status = Placeholder.  
*Source:* EC11 Roadmap

**Issue 4E.2 — Bulk-replace placeholders from final manifest [TBD — Design Required]**  
When manifest received: match names to placeholders and update records. Preserve flight/room links.  
*Open question:* Linking/matching field — by Role, sequential order, or manual match UI?  
*Source:* EC11 Roadmap

---

### Epic 4F — Artist Vehicle Management

**Issue 4F.1 — Create/assign artist vehicle from portal**  
From Travel tab drawer: create vehicle record in Artist Vehicles board (18412445612).  
Fields: Vehicle Type (Nightliner/Van/Truck/Other), Arrival Date, Vehicle in Cluj [timeline], Vehicle on Site [timeline — can overlap], Vehicle Parked At (Accommodation or Stages).  
*Source:* EC11 Roadmap

---

### Epic 4G — Itinerary Generation

**Issue 4G.1 — Contacts block in fixed order**  
Booking → Advancing Lead → Hotel (Front Desk from Accommodation board) → Angel. "TBC" if not yet assigned.  
*Source:* EC12 Team Feedback

**Issue 4G.2 — Auto-populate stage schedule**  
Load In, Soundcheck, Show pulled from Monday mirrors automatically. No manual input.  
*Source:* EC12 Team Feedback

**Issue 4G.3 — Pull transfers directly from Monday [TBD — Design Required]**  
Transfers section rendered from Transfers board filtered by `Artist @ Transfer`.  
*Open question:* Real-time read or snapshot strategy for itinerary generation?  
*Source:* EC12 Team Feedback

**Issue 4G.4 — Multi-stage artist display [TBD — Design Required]**  
Artists with multiple slots on different stages display correctly (separate day blocks, no duplication).  
*Open question:* How are multi-stage slots modelled? Multiple `Live Stage Slot` relations confirmed?  
*Source:* EC12 Team Feedback

**Issue 4G.5 — Remove pre-requested meals from itinerary output**  
Meals fields removed from itinerary sent to Tour Managers. Meals remain in Hospitality tab internally.  
*Source:* EC12 Team Feedback

---

### Epic 4H — Email Generation (magic.email)

**Issue 4H.1 — Phase-aware email draft with missing datapoints**  
Missing datapoints panel scans artist record and flags gaps per phase. "Insert missing items" button appends formatted list to draft. Cadence selector (First-touch / Chase / Confirm / Final Brief) determines which gaps are highlighted.  
*Source:* EC11 Roadmap — "email draft for the phase I'm in, that reminds me of missing datapoints"

---

### Epic 4I — Role-Based Views

**Issue 4I.1 — Angel View: artist timings in one place**  
Filtered to Angel's own artists. Columns: Artist, Show Day, Stage, Gate, Load In, Soundcheck, Show times, Hotel, Arrival Transfer. Read-only. Mobile-friendly.  
*Source:* EC11 Roadmap

**Issue 4I.2 — Backstage Manager View: arrivals/departures board + advancing summary + meals**  
Three panels: Arrivals (ETA, notes, party size), Departures, Advancing summary (Notes for BSM, Rider status, DR). Filters to their stage.  
*Source:* EC11 Roadmap + EC12 Team Feedback

**Issue 4I.3 — Hospitality Manager View: team progress + expected inputs**  
Panel A: team progress (all artists, Lead, status columns). Panel B: what hospitality is expected to provide (Catering Req, DR Setup, 00 Req, Overnight Food, Meals @ Hotel). Exportable.  
*Source:* EC11 Roadmap + EC12 Team Feedback

**Issue 4I.4 — Transport Coordinator View: flight delay dashboard**  
Shows flights where Revised Arrival Time ≠ Arrival Time. Per flight: artist, delta (minutes), affected transfers, drivers, angels. Affected transfer rows highlighted red if timing is critical.  
*Source:* EC11 Roadmap

---

### Epic 4J — Rider Tools [NTH]

**Issue 4J.1 — Rider Alternatives: extract rider to Google Sheet**  
Tool extracts rider content and structures it into a Google Sheet. Sheet auto-populates alternatives based on Artist Type, Origin. Riders Coordinator validates rather than inputs.  
*Source:* EC11 Roadmap

---

### Epic 4K — Meals Automation [NTH]

**Issue 4K.1 — Assign meal: create subitem record**  
On meal assignment, create subitem `[Artist Name] — [Meal Type]` under artist. Pulls through to Hospitality Manager meals summary.  
*Source:* EC11 Roadmap

---

## Open Questions (Design Required Before Implementation)

| # | Question | Blocks |
|---|----------|--------|
| Q1 | Auth method: Monday OAuth, email/password, or passkey? | Issue 1.3 |
| Q2 | Steps of Advancing document — where does it live / who owns it? | Issue 1.4 |
| Q3 | Transfers in itinerary: real-time read or snapshot? | Issue 4G.3 |
| Q4 | Multi-stage artists: multiple `Live Stage Slot` relations or separate field? | Issue 4G.4 |
| Q5 | Placeholder bulk-replace matching field: Role / sequential / manual UI? | Issue 4E.2 |
| Q6 | Google Drive auth: which service account owns the folder structure? | Issue 4C.1 |
| Q7 | Hosting: Vercel / self-hosted / other? Domain confirmed? | Issue 1.2 |

---

## Prioritised Delivery Order

| Priority | Layer / Issue | Rationale |
|----------|--------------|-----------|
| P0 | Layer 1 — All infrastructure | Nothing works without API connection + auth |
| P0 | Layer 2 — Design → code | Portal must look right before data lands |
| P0 | Layer 3 — Field wiring | All interactions depend on correct field mapping |
| P0 (pre-advancing) | Epic 4A — Board cleanup | Data integrity prerequisite |
| P1 | 4B (Progress bar), 4D (Flights/Routes), 4G (Itinerary) | Core advancing workflow |
| P1 | 4H (Email), 4E (Artist Party) | High daily use by Advancing Leads |
| P2 | 4I (Role views), 4F (Vehicles), 4C (KO Automation) | Enables other teams; Drive auth setup |
| NTH | 4J (Rider tools), 4K (Meals) | Valuable but not critical path |

---

## Verification

- **Layer 1:** Portal loads at domain over HTTPS. Login works. Monday API query returns real data.
- **Layer 2:** All nav routes load. Design matches V3 file. Mock data renders correctly in all views.
- **Layer 3:** Field map audit signed off. Each field in portal shows live Monday data. Edits save back.
- **Layer 4:** Each interaction tested end-to-end: KO creates folder + link, route generation writes Transfer record, itinerary renders without manual input, role views filter correctly per logged-in user.
- **Open questions resolved** before Issues 1.3, 4C.1, 4E.2, 4G.3, 4G.4 begin development.
