# EC12** Monday.com Board Structure - Complete Field Mapping

## Overview
Comprehensive event management system for Electric Castle 2024 with 10 interconnected Monday.com boards managing artists, logistics, accommodations, flights, transfers, and staffing.

---

## Board: EC12** - Accommodation (ID: 18412445605)
**Purpose**: Central hub for hotel information, amenities, and artist hotel allocations

| Field Name | Field ID | Field Type | Purpose | Input Method |
|---|---|---|---|---|
| Name | `name` | Key | Hotel name identifier | Text |
| Subitems | `subitems` | Connection | Links to Hotel Rooms board (18412445627) | Board Relation |
| Hotel Type | `label__1` | Input | Categories: Camping Access, Camping, ★★★, ★★★★, ★★★★★ | Status |
| Hotel Address | `long_text__1` | Input | Full street address | Long Text |
| Google Maps Pin | `text__1` | Input | Location link for mapping | Text |
| Front Desk Phone Number | `text5__1` | Input | Primary contact number | Text |
| Front Desk Email | `text7__1` | Input | Email contact | Text |
| Standard Check-in Time | `text8__1` | Input | Check-in policy (e.g., 3 PM) | Text |
| Standard Check-out Time | `text2__1` | Input | Check-out policy (e.g., 11 AM) | Text |
| Breakfast Available | `text3__1` | Input | Breakfast details & timing | Text |
| Restaurant Opening Times | `text0__1` | Input | Restaurant schedule | Text |
| Room Service Availability | `text4__1` | Input | Room service hours | Text |
| Wellness | `text9__1` | Input | Spa/wellness facilities | Text |
| Item ID | `item_id__1` | Key | System identifier | Auto-generated |
| Laundry | `text71__1` | Input | Laundry service details | Text |
| Hotel Website | `link__1` | Input | Web link | Link |
| Breakfast Timings | `long_text_mkswx8cv` | Input | Detailed breakfast schedule | Long Text |
| Hotel Car Park | `long_text_mkswytb5` | Input | Car park info & locations | Long Text |
| Artists - Artist Tracker @ Hotel | `connect_boards54` | Connection | Links to Artist Advancing Tracker (18412445614) | Board Relation |
| Total Artist Party per Hotel | `mirror5` | Connection | Mirrors formula from Artist Tracker | Mirror |
| Hotel Dates | `mirror8` | Connection | Mirrors Hotel Timeline from Artist Tracker | Mirror |
| Hotel Rooms | `board_relation_mkxdj428` | Connection | Links to Hotel Rooms board (18412445608) | Board Relation |

---

## Board: EC12** - Team Members (ID: 18412445606)
**Purpose**: Staff directory with department assignments and stage/artist roles

| Field Name | Field ID | Field Type | Purpose | Input Method |
|---|---|---|---|---|
| Name | `name` | Key | Staff member name | Text |
| Team | `label` | Input | 12 departments: Angel, Advancing, Backstage, Booking, Catering, Driver, Logistics, Procurement, Stage, Transport Team, Accommodation, Don't use | Status |
| Mobile No | `text7__1` | Input | Contact number | Text |
| Email | `email2__1` | Input | Email address | Email |
| Role | `text1__1` | Input | Job title/position | Text |
| Nickname | `text__1` | Input | Informal name | Text |
| Item ID | `item_id__1` | Key | System identifier | Auto-generated |
| EC Team @ Stage | `link_to_ec10___stages21__1` | Connection | Links staff to stage (Stages board 18412445613) | Board Relation |
| Artist @ Tracker | `board_relation_mm36e1fc` | Connection | Links staff to artists they manage (Artist Advancing Tracker 18412445614) | Board Relation |

---

## Board: EC12** - Flights (ID: 18412445607)
**Purpose**: Flight scheduling and airline management for artists and crew

| Field Name | Field ID | Field Type | Purpose | Input Method |
|---|---|---|---|---|
| Name | `name` | Key | Flight identifier | Text |
| Subitems | `subtasks_mksd4tdv` | Connection | Links to Flight Segments board (18412445628) | Board Relation |
| Status | `status` | Input | Working on it, Done, Stuck, NEW, Manual | Status |
| Flight Date | `date__1` | Input | Departure/arrival date | Date |
| Departure Time | `hour5__1` | Input | Scheduled departure | Hour |
| Arrival Time | `hour52__1` | Input | Scheduled arrival | Hour |
| Time @ CLJ | `hour__1` | Input | Time at Cluj airport | Hour |
| Arrival Method | `label__1` | Input | 30+ airlines: Wizz Air, Ryanair, Turkish, LOT, Private, Vehicle, etc. | Status |
| To/From | `text__1` | Input | Route origin/destination | Text |
| Route Type | `label8__1` | Input | Departure, Arrival, or Private | Status |
| Artist Arrival Flight | `board_relation__1` | Connection | Links to artists (Artist Advancing Tracker 18412445614) | Board Relation |
| Artist Departure Flight | `board_relation1__1` | Connection | Links to artists (Artist Advancing Tracker 18412445614) | Board Relation |
| Last Updated | `last_updated__1` | Key | Auto-timestamp | Auto-generated |
| Flight Notes | `long_text__1` | Input | Special requirements/amendments | Long Text |
| Revised Arrival Time | `hour_mkrynmed` | Input | Updated arrival if changed | Hour |
| Revised Departure Time | `hour_mkrydyfc` | Input | Updated departure if changed | Hour |
| Person Arrival Flight | `board_relation_mm2b2pvp` | Connection | Links individuals to flights (Artist Party 18412445609) | Board Relation |

---

## Board: EC12** - Hotel Rooms (ID: 18412445608)
**Purpose**: Room inventory and allocation tracking

| Field Name | Field ID | Field Type | Purpose | Input Method |
|---|---|---|---|---|
| Name | `name` | Key | Room identifier (e.g., "A101") | Text |
| Person | `person` | Input | Assigned guest | People |
| Status | `status` | Input | Working on it, Done, Stuck | Status |
| Hotel Door Number | `text_mm366mby` | Input | Room number | Text |
| Hotel Room Floor | `text_mm36nvxr` | Input | Floor location | Text |
| File | `file_mkxd25zw` | Input | Room photos/documentation | File |
| Bathtub | `boolean_mkxd9kmj` | Input | Amenity indicator | Checkbox |
| Balcony | `boolean_mkxd4rbg` | Input | Amenity indicator | Checkbox |
| Hotel | `board_relation_mkxdn254` | Connection | Links to hotel (Accommodation 18412445605) | Board Relation |
| Person in Room | `board_relation_mm2byzk4` | Connection | Links to person (Artist Party 18412445609) | Board Relation |
| Artist in Room | `lookup_mm2bnj1g` | Connection | Mirrors linked artist from party | Mirror |
| People Timeline | `lookup_mm2bmrk6` | Connection | Mirrors guest duration | Mirror |

---

## Board: EC12** - Artist Party (ID: 18412445609)
**Purpose**: Travel party member management with flight and accommodation linking

| Field Name | Field ID | Field Type | Purpose | Input Method |
|---|---|---|---|---|
| Name | `name` | Key | Party member name | Text |
| Artist Travel Party Member Role | `person_role` | Input | 27 roles: Artist, PM, TM, Photographer, Manager, Driver, Crew, Stylist, etc. | Dropdown |
| Artist Travel Party Member Email | `text__1` | Input | Contact email | Text |
| Artist Travel Party Member Phone | `phone_number__1` | Input | Contact number | Text |
| Artist Travel Party Member Status | `status` | Input | Pending, Confirmed, Cancelled, Unknown status | Status |
| Party Member Of | `board_relation_mksce4jt` | Connection | Links to artist (Artist Advancing Tracker 18412445614) | Board Relation |
| Artist Party Type | `lookup_mm2bvnhf` | Connection | Mirrors A/B/C Party classification | Mirror |
| Person - Arrival Flight | `board_relation_mm2b1dbp` | Connection | Links to arrival flight (Flights 18412445607) | Board Relation |
| Person - Arrival Flight Date | `lookup_mm2beamz` | Connection | Mirrors flight date | Mirror |
| Person - Departure Flight | `board_relation_mm2bt9hm` | Connection | Links to departure flight (Flights 18412445607) | Board Relation |
| Person - Departure Flight Date | `lookup_mm2b9fbh` | Connection | Mirrors flight date | Mirror |
| Person - Hotel Room | `board_relation_mm2bmd53` | Connection | Links to room (Hotel Rooms 18412445608) | Board Relation |
| Person - Hotel Timeline | `timerange_mm2bhjbp` | Input | Stay duration dates | Timeline |
| Person - Hotel Nights | `formula_mm2b4fnv` | Calculated | DAYS(End, Start) | Formula |

---

## Board: EC12** - Dressing Rooms (ID: 18412445610)
**Purpose**: Dressing room allocation and preferences

| Field Name | Field ID | Field Type | Purpose | Input Method |
|---|---|---|---|---|
| Name | `name` | Key | Dressing room identifier | Text |
| Artist @ Dressing Room | `board_relation__1` | Connection | Links to artist (Artist Advancing Tracker 18412445614) | Board Relation |
| Stage | `mirror6__1` | Connection | Mirrors stage assignment | Mirror |
| DR Requested | `mirror1__1` | Connection | Mirrors dressing room preferences | Mirror |
| Show Day | `mirror__1` | Connection | Mirrors show day (16/07-20/07) | Mirror |

---

## Board: EC12** - Transfers (ID: 18412445611)
**Purpose**: Ground transportation routing and logistics

| Field Name | Field ID | Field Type | Purpose | Input Method |
|---|---|---|---|---|
| Name | `name` | Key | Transfer identifier | Text |
| Transfer Status | `status__1` | Input | Auto-created, Has Date, Has Passengers, Route notes added, Complete, Must check, Placeholder, Published | Status |
| Artist @ Transfer | `board_relation__1` | Connection | Links to artist (Artist Advancing Tracker 18412445614) | Board Relation |
| Show Day | `mirror120__1` | Connection | Mirrors show day | Mirror |
| Stage | `mirror3__1` | Connection | Mirrors stage assignment | Mirror |
| Route Type | `label__1` | Input | 30+ options: Airport-Hotel, Hotel-Show, Show-Airport, Load-in, Soundcheck-Hotel, etc. | Status |
| Route Date | `date__1` | Input | When transfer needed | Date |
| Passengers | `numbers__1` | Input | Headcount | Number |
| Route Notes | `long_text__1` | Input | Special instructions | Long Text |
| Route Cargo | `long_text6__1` | Input | Equipment/materials being transported | Long Text |
| Flight Associated with Route | `text__1` | Input | Flight number reference | Text |
| Flight Time @ CLJ | `text9__1` | Input | Timing coordination | Text |
| Pickup | `label7__1` | Input | Starting location | Status |
| Dropoff | `label70__1` | Input | Destination location | Status |
| Festival Gate | `mirror7__1` | Connection | Mirrors gate assignment | Mirror |
| Signage | `mirror29__1` | Connection | Mirrors signage notes | Mirror |
| Angel | `mirror12__1` | Connection | Mirrors angel assignment | Mirror |
| Angel Phone | `mirror97__1` | Connection | Mirrors angel contact | Mirror |
| Cargo | `mirror__1` | Connection | Mirrors equipment details | Mirror |
| Arrival Flight Date | `mirror2__1` | Connection | Mirrors flight info | Mirror |
| Arrival Flight No | `mirror5__1` | Connection | Mirrors flight identifier | Mirror |
| Departure Flight Date | `mirror1__1` | Connection | Mirrors flight info | Mirror |
| Departure Flight No | `mirror91__1` | Connection | Mirrors flight identifier | Mirror |
| Vehicle Preference | `mirror9__1` | Connection | Mirrors vehicle type request | Mirror |
| Transfers Requested | `mirror0__1` | Connection | Mirrors transfer requirements | Mirror |
| Hotel | `mirror4__1` | Connection | Mirrors hotel assignment | Mirror |
| Route Requested By | `people__1` | Input | Staff requesting route | People |
| Item ID | `item_id__1` | Key | System identifier | Auto-generated |
| Google Sheets Row UUID | `long_text2__1` | Input | Automation/sync reference | Text |

---

## Board: EC12** - Artist Vehicles (ID: 18412445612)
**Purpose**: Vehicle management and parking coordination

| Field Name | Field ID | Field Type | Purpose | Input Method |
|---|---|---|---|---|
| Name | `name` | Key | Vehicle identifier | Text |
| Person | `person` | Input | Vehicle assigned to | People |
| Status | `status` | Input | Working on it, Done, Stuck | Status |
| Date | `date4` | Input | Vehicle-related date | Date |
| Vehicle in Cluj | `timerange_mm14jgq4` | Input | Time in city | Timeline |
| Vehicle on Site | `timerange_mm14pjqm` | Input | Time at festival | Timeline |
| Artist's Vehicle | `board_relation_mm14a1th` | Connection | Links to artist (Artist Advancing Tracker 18412445614) | Board Relation |
| Vehicle Type | `color_mm145nvg` | Input | Van or Truck | Status |
| Vehicle Parked At | `board_relation_mm14fz4v` | Connection | Parking location (Accommodation 18412445605, Stages 18412445613) | Board Relation |
| Car Park Notes | `lookup_mm145k0g` | Connection | Mirrors parking details | Mirror |

---

## Board: EC12** - Stages (ID: 18412445613)
**Purpose**: Stage configuration, capacity, and hospitality details

| Field Name | Field ID | Field Type | Purpose | Input Method |
|---|---|---|---|---|
| Name | `name` | Key | Stage identifier | Text |
| Artist (EC12) | `board_relation_mm14903` | Connection | Links to artists (Artist Advancing Tracker 18412445614) | Board Relation |
| Stage Sponsored By | `text` | Input | Sponsor name | Text |
| Capacity | `numbers` | Input | Audience capacity | Number |
| H: A La Carte Menu | `a_la_carte_menu` | Input | Order for takeaway, Served to table, Buffet Style | Dropdown |
| H: Platter | `dropdown74` | Input | Made to order or Daily order | Dropdown |
| H: Overnight Food | `dropdown3` | Input | Food Court, Pizza, Platters, Menu | Dropdown |
| H: Lounge | `dropdown8` | Input | Lounge facilities | Dropdown |
| H: Fridge | `dropdown9` | Input | DR, Bar, Storage, Team | Dropdown |
| H: Drinks | `dropdown89` | Input | Staffed Bar, Stage Fridge, Wristband, Rider only | Dropdown |
| Stage Index | `numbers__1` | Input | Stage ordering number | Number |
| Item ID | `item_id__1` | Key | System identifier | Auto-generated |
| Link to Standard Drive Pack | `text__1` | Input | Technical documentation link | Text |
| Festival Gate | `text4__1` | Input | Gate entry point | Text |
| Team Members @ Stage | `connect_boards2__1` | Connection | Links staff (Team Members 18412445606) | Board Relation |
| Stage Info Status | `color_mkpgw72a` | Input | Confirm Details, Confirmed, Stuck | Status |

---

## Board: EC12** - Artist Advancing Tracker (ID: 18412445614)
**Purpose**: Central hub for all artist information and advancing coordination

| Field Name | Field ID | Field Type | Purpose | Input Method |
|---|---|---|---|---|
| Name | `name` | Key | Artist/group name | Text |
| Subitems | `subitems` | Connection | Links to Tasks board (18412445626) | Board Relation |
| Advancing Lead | `person` | Input | Responsible staff member | People |
| Advancing Status | `status` | Input | 15 states: No action taken, Waiting on them, Waiting on me, Time to chase, Next Step, Missing Info, Done, New Entry, Reply ASAP, Apply Templates, Get Artist Details, Show cancelled, Advancing KO, Chased, blank | Status |
| Artist Party Type | `dropdown_mm2bjhn9` | Input | A Party, B Party, C Party, Other | Dropdown |
| Show Day | `dropdown1__1` | Input | 16/07 Day 1 - 20/07 Day 5 | Dropdown |
| Live Stage Slot | `board_relation__1` | Connection | Links to Stage Timings board (18390343044) | Board Relation |
| Stage Name (from Timings) | `lookup_mksehn57` | Connection | Mirrors stage from timings | Mirror |
| Stage Slot Type | `mirror38__1` | Connection | Mirrors slot classification | Mirror |
| Duration (m) | `mirror64__1` | Connection | Mirrors set length | Mirror |
| Show Start Time | `mirror45__1` | Connection | Mirrors performance start | Mirror |
| Show End Time | `mirror901__1` | Connection | Mirrors performance end | Mirror |
| Soundcheck Slot | `connect_boards235__1` | Connection | Links to Soundcheck Timings board (18390342430) | Board Relation |
| Soundcheck Stage Slot Type | `mirror57__1` | Connection | Mirrors soundcheck classification | Mirror |
| Soundcheck Duration (m) | `mirror8__1` | Connection | Mirrors soundcheck length | Mirror |
| Soundcheck Starts | `mirror761__1` | Connection | Mirrors soundcheck start | Mirror |
| Soundcheck Ends | `mirror464__1` | Connection | Mirrors soundcheck end | Mirror |
| Artist Type | `dropdown9` | Input | 1. Headliner, 2. Co-Headliner, 3. Support Act, 4. Standard, 5. Other, 6. VIP | Dropdown |
| Actions Completed | `dropdown` | Input | 39-item multi-select: Brochure, Rider, Flights, Hotel, Transport, DR, etc. | Dropdown |
| Item ID | `item_id` | Key | System identifier | Auto-generated |
| Stage (Corrected) | `board_relation3` | Connection | Links to stage (Stages 18412445613) | Board Relation |
| Festival Gate | `mirror2__1` | Connection | Mirrors gate assignment | Mirror |
| Stage Index | `mirror0__1` | Connection | Mirrors stage ordering | Mirror |
| Artist Origin | `artist_origin__1` | Input | INT (International), RO (Romania), Local, SKIP! | Status |
| Hotel Timeline | `timeline5` | Input | Accommodation dates | Timeline |
| Hotel Nights | `formula_mksc17p1` | Calculated | DAYS(End, Start) | Formula |
| Related Artist/Crew | `connect_boards9__1` | Connection | Links other related artists | Board Relation |
| Nights Paid by EC | `numbers_16` | Input | EC-funded accommodation nights | Number |
| Nights Paid by Artist | `formula4` | Calculated | Total nights - EC nights | Formula |
| A Party Size | `numbers` | Input | Tier A party headcount | Number |
| B Party Size | `numeric_mksczbrh` | Input | Tier B party headcount | Number |
| C Party Size | `numbers3` | Input | Tier C party headcount | Number |
| Other Travel Party | `numbers__1` | Input | Additional entourage | Number |
| Travel Party Total | `formula` | Calculated | A+B+C+Other | Formula |
| Hotel Notes | `hotel_notes` | Input | Accommodation details | Long Text |
| Rooming List | `rooming_list` | Input | Room assignments | Long Text |
| A Party Hotel | `board_relation7` | Connection | Links to accommodation (Accommodation 18412445605) | Board Relation |
| B Party Hotel | `board_relation_mm14c373` | Connection | Links to accommodation (Accommodation 18412445605) | Board Relation |
| C Party Hotel | `board_relation35` | Connection | Links to accommodation (Accommodation 18412445605) | Board Relation |
| Artist Pack Link | `link__1` | Input | External documentation link | Link |
| Travel Party Names - Text | `long_text__1` | Input | Internal reference (marked for deletion) | Long Text |
| Req Level | `label__1` | Input | Chill, 💅, 💅💅, 💅💅💅 | Status |

---

## Connection Architecture

### Primary Hub
**Artist Advancing Tracker** (18412445614) - Central repository connecting to all operational boards:
- ✓ Flights (Arrival & Departure)
- ✓ Accommodations (3-tier hotel allocation)
- ✓ Dressing Rooms
- ✓ Transfers
- ✓ Stages (Performance & Soundcheck timings)
- ✓ Artist Party (travel entourage)
- ✓ Vehicles
- ✓ Team Members (advancing leads)
- ✓ Hotel Rooms (allocation verification)

### Secondary Hubs
**Accommodation** → Operational support
- Hotel Rooms inventory
- Artist Party tracking
- Vehicle parking coordination

**Flights** → Cross-board logistics
- Artist Party linking
- Transfer coordination

**Stages** → Performance coordination
- Team assignments
- Artist performance details

---

## Data Flow Summary

1. **Artist Entry Point**: Artist Advancing Tracker (primary record)
2. **Travel Logistics**: Flights → Artist Party → Hotel Rooms ↔ Hotel Timeline
3. **Ground Movement**: Transfers (pulls data via mirrors)
4. **Venue Logistics**: Stages → Team @ Stage, Dressing Rooms
5. **Equipment**: Artist Vehicles → Parking coordination
6. **Staffing**: Team Members (department assignments & role tracking)

---

## Notes for Refinement

- 10 interconnected boards managing complete artist lifecycle
- Heavy use of Mirror fields for real-time data sync
- Formula fields for automated calculations (nights, party size)
- Status fields for workflow tracking
- Multiple connection types: Board Relations, Subtasks, Mirror columns
- Supports multi-tier accommodation (A/B/C parties)
- Integration with external Stage Timings & Soundcheck boards
