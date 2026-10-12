# BakulUM

**Isi bakul, jimat belanja.** (Fill the basket, save money.)

BakulUM is an Android app for Universiti Malaya (UM). Campus cafeteria, kiosk and minimart operators post unsold meals and near-expiry groceries, and students reserve them at a discount, cutting campus food waste and giving students cheaper food.

> **Status:** planning and early development. Group project for **WIA2007 Mobile Application Development**, Semester 1, 2026/2027. Sections marked _(planned)_ describe the design, not finished code.

**SDG:** 12 (Responsible Consumption and Production), target 12.3, with links to SDG 2 and SDG 13.

---

## The problem

UM cafeterias, kiosks, college canteens and minimarts may end the day with unsold food while students on tight budgets pay full price elsewhere. This is a working hypothesis that the team is validating through operator interviews, a student survey and contact with a UM unit. Findings will be added to the project report.

## Who it is for

| Stakeholder | Role in the app |
|---|---|
| Cafeteria, kiosk and minimart operators | Post surplus meals and near-expiry groceries |
| Students (residential and commuter) | Browse, reserve and collect |
| Residential college management, Sustainability Centre, student affairs office, JPPHB | Validate the problem, guide rules and any pilot |
| Cleaning contractors | Benefit from less food waste |

UM office names are assumptions until confirmed.

## Features _(planned)_

Six modules, three functional requirements each.

1. **Authentication and roles:** register and log in with a UM email, reset password, student, vendor and admin roles (admin approves vendors).
2. **Vendor surplus posting:** create listings (item, category, quantity, price or free, halal declaration, best-before or expiry time, pickup window, photo), edit or cancel, and see history.
3. **Student browse and reserve:** browse and filter listings, reserve portions, cancel before the cut-off.
4. **Pickup and notifications:** pickup code or QR per reservation, vendor verifies and marks collected, local notifications and reminders.
5. **Impact tracker and admin:** meals saved and estimated money saved, per-outlet totals, admin tools and CSV export.
6. **AI label scan:** the vendor photographs a grocery label, on-device text recognition reads it, a parser finds the best-before or expiry date, the vendor confirms it, and expired items cannot be posted.

### Non-functional requirements

- **Performance:** the listings screen loads within 3 seconds with 1,000 stored listings on a mid-range Android phone.
- **Reservation integrity:** the app never confirms more portions than were posted, even when reservations happen at the same time.

## Tech stack

| Layer | Choice |
|---|---|
| Platform | Native Android, Kotlin, Android Studio |
| Database | Room (SQLite) on the device |
| Accounts | Local accounts with hashed passwords |
| Notifications | Android local notifications (WorkManager) |
| Images | App internal storage |
| AI | Google ML Kit text recognition, bundled model, on device |
| Collaboration | GitHub, Trello |

### Architecture _(planned)_

The app reads and writes data only through repository interfaces, for example `ListingRepository`, `ReservationRepository` and `AccountRepository`. A Room implementation is built now. Records use UUID ids and an updated-at timestamp, so a Supabase implementation can be added later without changing the screens.

**Limitation:** data is stored on one device. The vendor and student roles are shown on the same phone or emulator using seeded sample data. Cross-device sharing, campus-wide totals and real multi-user load need a backend and are future work.

## Getting started

### Requirements

- Android Studio (latest stable version)
- JDK bundled with Android Studio
- An Android emulator or a phone with Android 6.0 (API 23) or higher

### Inspect the database

Use **App Inspection > Database Inspector** in Android Studio while the app runs to view and query tables.

## Project structure _(planned)_

```
app/
  src/main/java/.../
    data/         Room entities, DAOs, database, repository implementations
    domain/       repository interfaces and models
    ui/           screens and view models, grouped by module
    ai/           label scan and date parser
  src/main/assets/
    seed/         sample data loaded on first run
schemas/          exported Room schema JSON
docs/             requirements, design notes, test results
```

## Team workflow

- Work on a **feature branch** and open a **pull request**. At least one teammate reviews it before merge. Do not push directly to `main`.
- **Schema changes** (entities, DAOs, database version) go through the data layer lead. Request one with an issue or Trello card, and the lead adds it in a small pull request. Rebase your branch after it merges.
- Build screens against the repository **interfaces**, using a fake in-memory version until the Room version is ready.
- While developing, Room may wipe and rebuild the database on schema change. Reinstall or clear app data after pulling a schema change.
- Do **not** commit `.db` files, `build/` or `local.properties`.
- Keep Trello cards and commit messages clear, since commit logs and board reports are used as contribution evidence.

## Team

| Role | Member | Main responsibilities |
|---|---|---|
| Project manager and stakeholder lead | _TBD_ | Plan, Trello, deadlines, interviews, video lead, admin screens |
| Software architect and data layer lead | _TBD_ | Room schema, repository interfaces, reservation transaction, code review |
| Lead developer, student app | _TBD_ | Browse, reserve, student side of pickup |
| Developer, vendor app | _TBD_ | Posting, vendor side of pickup, label scan |
| UI/UX designer and developer | _TBD_ | Wireframes, colour scheme, accessibility, login screens, impact view |
| Tester and data lead | _TBD_ | Test plan, survey analysis, user testing, NFR and AI accuracy tests, dashboard and CSV export |

## Timeline

| Date | Milestone |
|---|---|
| 9 Oct 2026 | Project start |
| Late Oct to mid Nov 2026 | Interviews, survey, wireframes, schema |
| Mid Nov to 20 Dec 2026 | Build: core flow, then pickup, impact and label scan |
| 6 Dec 2026 | Core flow and label scan prototype working |
| 14 to 27 Dec 2026 | Testing and user testing |
| 12 pm, 3 Jan 2027 | Video due |
| 12 pm, 10 Jan 2027 | Report due |

Dates may change once the UM calendar is checked.

## AI usage

AI is used in two ways:

1. **In the product:** the label scan uses on-device text recognition (no API key, no internet).
2. **In development:** teammates may use AI tools to brainstorm, explain and refine code. Every use is recorded in [`docs/AI_USAGE_LOG.md`](docs/AI_USAGE_LOG.md) with the tool, the prompt, and how the output was tested and changed, as the course requires.

## Future work

- Supabase backend (free plan) for shared data between vendor and student phones, with server push
- iOS version through a cross-platform rewrite
- Vendor sales analytics and suggested quantities and prices
- Natural-language search in English and Malay
- Real pilot with one or two UM outlets

## Course and licence

Universiti Malaya, WIA2007 Mobile Application Development, group project 2026/2027 Semester 1. Academic project, so no licence is granted yet. Add one if the project is made public.
