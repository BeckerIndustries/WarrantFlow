# Changelog

All notable changes to **WarrantFlow** are recorded here. Versions follow
[Semantic Versioning](https://semver.org/) (MAJOR.MINOR.PATCH).

The latest .exe is always at the top of the
[Releases page](https://github.com/BeckerIndustries/WarrantFlow/releases).

## [Unreleased]
- Bundled `search_warrant.docx` and `warrant_return.docx` inside the .exe.
  First launch in an empty folder now writes both templates to disk
  automatically — no more "drop the templates folder next to the exe"
  setup step. Your edited copies are never overwritten on subsequent
  launches.

## [1.0.0] — 2026-05-30

First public release.

### Search warrant
- Form-driven editor for every CA PC 1524(a) statutory ground (all 22),
  with conditional template rendering so unchecked grounds drop out
  entirely.
- "Find Code" picker over the bundled California statutes catalog.
  Codes insert with long-form names (`Penal Code 459`, not `PC 459`).
- Per-officer profile: name, badge, rank, agency, station, county, hire
  date, assignments, standing expertise statement.
- Signature capture (mouse or touchscreen) — embeds on every generated
  document.
- Special court orders: electronic monitoring, sealing, night service,
  delayed notification, non-disclosure, Hobbs sealing, 10-day return
  waiver. Each with its own per-order justification text.
- Snippet library: Google account language, iCloud language, cellphone
  search language, etc., refreshed from the public catalog at
  https://github.com/BeckerIndustries/WarrantFlow/blob/main/catalog.json.

### Warrant return
- Focused short form: executor name + rank (prefilled), issuing judicial
  officer + court (defaults to "Superior Court of California, County of
  San Bernardino"), dates of issuance and execution, manner of service
  (Personal / Mail / Fax / Digital), locations searched, items seized.

### Output
- Generate as .docx or PDF. PDF requires Microsoft Word on the local
  machine.
- Output saves under `data\output\`; Save-As dialog lets the officer
  put it anywhere.

### Security / privacy
- AES-256-GCM vault, key derived via PBKDF2-SHA256 with 600,000
  iterations. Wrong-password = decrypt fails (no plaintext leak).
- Three wrong attempts in one session locks the app.
- Strong-password policy: minimum 8 characters with at least 2 each of
  uppercase, lowercase, digits, and special characters.
- Encrypted .swdb backups with the same master password.
- Auto-logout after configurable inactivity (default 10 minutes).
- No cloud sync, no telemetry. Only network call is the catalog refresh,
  triggered by the user.

### UX
- Dark and light themes; toggle in the title bar.
- Touch-friendly: 16pt body text, 44+ px touch targets, large nav buttons.
- Bookmarks page with shared catalog + user-added local links; network-
  restricted badge for intranet-only links.
- Home dashboard with update banner, supporter shoutouts, and
  announcements pulled from the public catalog.
- As-is disclaimer accepted on first launch and available any time
  under About → View Disclaimer.

[Unreleased]: https://github.com/BeckerIndustries/WarrantFlow/compare/v1.0.0...HEAD
[1.0.0]: https://github.com/BeckerIndustries/WarrantFlow/releases/tag/v1.0.0
