# Changelog

All notable changes to **WarrantFlow** are recorded here. Versions follow
[Semantic Versioning](https://semver.org/) (MAJOR.MINOR.PATCH).

The latest .exe is always at the top of the
[Releases page](https://github.com/BeckerIndustries/WarrantFlow/releases).

## [Unreleased]

## [1.0.5] — 2026-09-02

### Added
- **Live password checklist.** Setup and Settings → Change Password now
  show each strength rule with a red ✗ / green ✓ that updates as you
  type, plus a "Passwords match" indicator under the confirm box. The
  Create Vault / Change Password button stays disabled until everything
  is green, so there's no more guessing what's wrong after clicking.
- **First Name and Last Name fields** on the profile replace the single
  Full Name box, so every generated document prints the affiant as
  "First Last" no matter how it was typed. Names are tidied when you
  leave the box ("john smith" → "John Smith", "mccorry" → "McCorry");
  anything typed with deliberate capitals ("McCorry", "O'Brien",
  "van der Berg") is kept exactly. Existing profiles migrate
  automatically ("Smith, John" and "John Smith" both split correctly —
  check the Profile page once after updating). New template tokens
  `{{AFFIANT_FIRST_NAME}}` and `{{AFFIANT_LAST_NAME}}`.

## [1.0.4] — 2026-09-02

### Fixed
- **Declaration date on the warrant return printed in the machine's
  locale format** (`5/30/2026` on US machines, `30/05/2026` elsewhere).
  It now prints `05-30-2026` like every other date. New tokens
  `{{DECLARATION_DATE}}` (full date) and `{{DECLARATION_DAY_OF_MONTH}}`
  (just the day) were added, and the bundled `warrant_return.docx` now
  uses `{{DECLARATION_DATE}}`. `{{DECLARATION_DAY}}` keeps working as
  the full date, so a customized template from an earlier version prints
  the same thing. Existing installs keep their on-disk template as
  always; delete `templates\warrant_return.docx` to pick up the new copy.
- **"Discard unsaved changes?" no longer appears after only expanding the
  statutory-grounds list** in the warrant editor.
- **Restore From Backup keeps the previous vault** as `data\vault.swdb.bak`
  instead of silently overwriting it, and fully logs out before returning
  to the Unlock screen. Wipe / Reset removes the safety copy too.
- **Change Password now enforces the same strength rules as first-run
  Setup** (8+ characters, 2 each of upper, lower, digit, special). It
  previously accepted any 8-character password.

## [1.0.3] — 2026-05-30

### Changed
- **Catalog files moved into `catalogs/` folder** in the public repo
  so the repo root reads as just README, LICENSE, CHANGELOG, plus the
  catalogs and docs folders. The four default URLs (snippets,
  bookmarks, statutes, home) baked into the .exe were updated to
  match. No user-visible behavior change beyond a cleaner repo layout.

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

[Unreleased]: https://github.com/BeckerIndustries/WarrantFlow/compare/v1.0.5...HEAD
[1.0.5]: https://github.com/BeckerIndustries/WarrantFlow/releases/tag/v1.0.5
[1.0.4]: https://github.com/BeckerIndustries/WarrantFlow/releases/tag/v1.0.4
[1.0.3]: https://github.com/BeckerIndustries/WarrantFlow/releases/tag/v1.0.3
[1.0.2]: https://github.com/BeckerIndustries/WarrantFlow/releases/tag/v1.0.2
[1.0.1]: https://github.com/BeckerIndustries/WarrantFlow/releases/tag/v1.0.1
[1.0.0]: https://github.com/BeckerIndustries/WarrantFlow/releases/tag/v1.0.0
