# UMRefill

**Find the nearest working water refill point at Universiti Malaya, and cut single-use plastic.**

UMRefill is an Android app for the UM community. Pick a faculty and a floor, and see a 2D floor plan with the refill points pinned on it, the nearest working one highlighted. Users can report a station that is broken, empty or dirty, so the map stays trustworthy.

> **Status:** planning and early development. Group project for **WIA2007 Mobile Application Development**, Semester 1, 2026/2027. Sections marked _(planned)_ describe the design, not finished code.

**SDG:** 6 (Clean Water and Sanitation), targets 6.1 (safe drinking water access) and 6.b (community participation in water management).

---

## The problem

Students and staff often don't know where the nearest working fountain or refill point is in a given faculty, so they buy bottled water instead. Broken, switched-off or dirty fountains make it worse, with no way to know a station works before walking to it. This is a working hypothesis the team is validating through building walk-throughs, a student survey and contact with JPPHB or the Sustainability Centre.

## Signature feature: the 2D floor guide

Four taps: **pick faculty → pick floor → 2D floor plan with pinned points → nearest working point highlighted.**

This is a static floor-plan viewer with pins, **not** live indoor turn-by-turn navigation:
- Each floor is a simple 2D plan (traced by the team), stored as an image with points placed by coordinates.
- "Nearest" is measured from a reference point the user picks (entrance, lift or stairs), not live GPS, which does not work reliably indoors.
- "Directions" are a pin on the plan plus a short note ("Level 2, left of the lifts").

## Features _(planned)_

Five modules, three functional requirements each.

1. **Access and profile:** browse without login; UM email login for reporting and favourites; normal-user and admin roles.
2. **Faculty and floor browser:** pick faculty then floor, list points with location notes, filter by status and type.
3. **2D floor guide:** floor plan with pinned points, nearest working point highlighted, tap a pin for type, status and last-confirmed date.
4. **Community reporting:** report a point broken, empty, dirty or working, with optional photo; status updates; admin review.
5. **Impact and admin:** personal and campus counters of refills and bottles avoided; admin manages points, floor plans and CSV export.

### Non-functional requirements

- **Usability:** a first-time user finds the nearest working point in a chosen faculty within 3 taps and under 10 seconds, no login.
- **Map performance:** a floor plan with pins loads and renders within 2 seconds on a mid-range Android phone.

## Tech stack

| Layer | Choice |
|---|---|
| Platform | Native Android, Kotlin, Android Studio |
| Database | Room (SQLite) on the device |
| Accounts | Local accounts, hashed passwords; browsing needs no login |
| Floor plans | Images bundled in the app, points placed by coordinates |
| Images | App internal storage |
| Collaboration | GitHub, Trello |

### Architecture _(planned)_

The app reads and writes data only through repository interfaces, for example `RefillPointRepository` and `ReportRepository`. A Room implementation is built now. A shared backend (such as the Supabase free plan) can be added later without changing the screens, so a report from one phone becomes visible to everyone.

**Limitation:** data is stored on one device. Reports are not shared between phones, and the demo uses seeded sample data with the normal-user and admin roles on one device. Cross-device sharing is future work.

## Getting started

### Requirements

- Android Studio (latest stable version)
- An Android emulator or a phone with Android 6.0 (API 23) or higher

### Run the app

```bash
git clone https://github.com/<your-org>/umrefill.git
cd umrefill
```

1. Open the project folder in Android Studio and let Gradle sync.
2. Select an emulator or connected phone.
3. Press **Run**.

On first launch the app seeds sample faculties, floors, refill points and reports, so everyone sees the same demo data. To reset, clear the app's storage or reinstall.

## Project structure _(planned)_

```
app/
  src/main/java/.../
    data/         Room entities, DAOs, database, repository implementations
    domain/       repository interfaces and models
    ui/           screens and view models, grouped by module
    floorguide/   floor-plan rendering, pins, nearest-point logic
  src/main/assets/
    floors/       floor-plan images per faculty and floor
    seed/         sample data loaded on first run
schemas/          exported Room schema JSON
docs/             requirements, design notes, test results
```

## Team workflow

- Work on a **feature branch** and open a **pull request**. At least one teammate reviews before merge. No direct pushes to `main`.
- **Schema changes** (entities, DAOs, database version) go through the data layer lead via an issue or Trello card, in a small PR. Rebase after it merges.
- Build screens against the repository **interfaces**, using a fake in-memory version until Room is ready.
- While developing, Room may wipe and rebuild on schema change. Reinstall or clear app data after pulling a schema change.
- Do **not** commit `.db` files, `build/` or `local.properties`.
- Keep Trello cards and commit messages clear, since commit logs and board reports are used as contribution evidence.

## Team

| Role | Member | Main responsibilities |
|---|---|---|
| Project manager and stakeholder lead | _TBD_ | Plan, Trello, deadlines, JPPHB and faculty contacts, building surveys, video lead |
| Software architect and data layer lead | _TBD_ | Room schema, repository interfaces, integration, code review |
| Floor-guide developer | _TBD_ | 2D floor plan rendering, pins, nearest-point logic |
| Lead developer, browse and reporting | _TBD_ | Faculty and floor browser, report flow |
| UI/UX designer and developer | _TBD_ | Wireframes, colour scheme, accessibility, floor-plan assets |
| Tester and data lead | _TBD_ | Test plan, survey analysis, user testing, NFR measurements, dashboard and CSV export |

## Timeline

| Date | Milestone |
|---|---|
| 23 Oct 2026 | Project start |
| Late Oct to mid Nov 2026 | Building surveys, student survey, wireframes, floor plans, schema |
| Mid Nov to 20 Dec 2026 | Build: browse and floor guide, then reporting and impact |
| 6 Dec 2026 | Floor guide and core browse working for one faculty |
| 14 to 27 Dec 2026 | Testing and user testing |
| 12 pm, 3 Jan 2027 | Video due |
| 12 pm, 10 Jan 2027 | Report due |

Dates may change once the UM calendar is checked.

## AI usage

Teammates may use AI tools to brainstorm, explain and refine code. Every use is recorded in [`docs/AI_USAGE_LOG.md`](docs/AI_USAGE_LOG.md) with the tool, the prompt, and how the output was tested and changed, as the course requires.

## Future work

- Shared backend (Supabase free plan) so reports sync between phones, with push alerts for fixed stations
- Map all faculties and floors, not just the demo buildings
- iOS version through a cross-platform rewrite
- Live indoor positioning (Bluetooth beacons) for true nearest-point directions

## Course and licence

Universiti Malaya, WIA2007 Mobile Application Development, group project 2026/2027 Semester 1. Academic project, so no licence is granted yet.
