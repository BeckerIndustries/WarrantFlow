# Changelog

All notable changes to **WarrantFlow** are recorded here. Versions follow
[Semantic Versioning](https://semver.org/) (MAJOR.MINOR.PATCH).

The latest .exe is always at the top of the
[Releases page](https://github.com/BeckerIndustries/WarrantFlow/releases).

## [Unreleased]

## [1.0.2] — 2026-05-30

### Changed
- **Warrant Number is now on the warrant return form only.** It was
  showing up on the new search warrant editor too, but the
  court-assigned number isn't issued until the magistrate signs. The
  Document Details card now has Case Number + Title on a new search
  warrant, and Case Number + Warrant Number + Title on a return.
- **Logging is per-day with 90-day retention.** Logs roll into
  `data\logs\warrantflow-YYYY-MM-DD.log` instead of one growing file.
  Files older than 90 days are deleted on startup. User actions
  (unlock, generate, save, backup, restore, password change, vault
  wipe, manual log out, inactivity timeout) are now recorded with an
  `ACTION:` prefix for fast grep during incident review.
- **Production folder layout simplified.** The `README.txt` that used
  to sit next to the .exe is gone — the in-app About / View Disclaimer
  cover what officers need, and the rest lives on GitHub. In its place,
  a marker file named `! DO NOT TOUCH.txt` (leading "!" sorts it to
  the top in Explorer) drops into both `templates\` and `data\` on
  first launch with a one-paragraph warning to leave the contents
  alone.

## [1.0.1] — 2026-05-30

### Changed
- **Truly self-contained .exe.** Bundled `search_warrant.docx`,
  `warrant_return.docx`, and the daily-use `README.txt` inside the .exe.
  First launch in an empty folder now writes everything to disk
  automatically — no more "drop the templates folder next to the exe"
  setup step. Customized templates and READMEs are never overwritten on
  subsequent launches.

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

[Unreleased]: https://github.com/BeckerIndustries/WarrantFlow/compare/v1.0.2...HEAD
[1.0.2]: https://github.com/BeckerIndustries/WarrantFlow/releases/tag/v1.0.2
[1.0.1]: https://github.com/BeckerIndustries/WarrantFlow/releases/tag/v1.0.1
[1.0.0]: https://github.com/BeckerIndustries/WarrantFlow/releases/tag/v1.0.0
