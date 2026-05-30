# Catalog Reference

This document describes the four JSON catalogs hosted at the root of the
[BeckerIndustries/WarrantFlow](https://github.com/BeckerIndustries/WarrantFlow)
repo. The WarrantFlow desktop app fetches each one on demand and caches
the result locally so the app keeps working without an internet
connection.

| File | What it powers |
|---|---|
| `catalog.json` | Language Library (places, items, expertise paragraphs, probable-cause fragments) |
| `bookmarks.json` | Bookmarks page (court forms, DMV lookups, agency intranet links) |
| `california_statutes.json` | "Find Code" picker on the warrant editor |
| `home.json` | Home-dashboard banner, supporter shoutouts, update notice |

All four catalogs are **public and unauthenticated** — don't put anything
sensitive or agency-confidential in them. These are generic, reusable
resources.

---

## `catalog.json` — Language Library

```json
{
  "version": 1,
  "updated": "2026-05-27",
  "snippets": [
    {
      "id": "unique-stable-id",
      "category": "item",
      "title": "Human-readable title",
      "body": "The clause text. Use [BRACKETS] for fill-in blanks.",
      "tags": ["search", "keywords"]
    }
  ]
}
```

### Categories (exactly one of these)

| value | meaning |
|---|---|
| `place` | Places to be searched (residence, vehicle, account) |
| `item` | Items / property / records to be seized |
| `expertise` | Affiant-expertise boilerplate paragraphs |
| `probable_cause_fragment` | Reusable probable-cause language |
| `other` | Anything else |

### How to add or edit a clause

1. Edit `catalog.json`.
2. Keep `id` values stable and unique. Add new IDs for new clauses; don't
   recycle an old ID for different content.
3. Bump the top-level `version` and update `updated`.
4. Validate the JSON (`python -m json.tool catalog.json`, or any
   JSON linter).
5. Commit and push. The app picks up changes the next time a user clicks
   **Refresh** in the Language Library.

`[BRACKETED]` placeholders are deliberately left for the officer to fill
in after inserting the clause.

---

## `bookmarks.json` — Bookmarks page

```json
{
  "version": 1,
  "updated": "2026-05-27",
  "bookmarks": [
    {
      "id": "unique-stable-id",
      "title": "Human-readable title",
      "url": "https://example.gov/",
      "info": "Optional notes, e.g. 'Only works on the Sheriff's network'.",
      "category": "legal",
      "tags": ["search", "keywords"],
      "network_restricted": false
    }
  ]
}
```

- Only `http://` and `https://` URLs open from the app.
- `category` must be one of: `records`, `legal`, `mapping`, `agency`,
  `training`, `other`.
- `network_restricted` is optional and defaults to `false`. Set `true` for
  links that only resolve on the agency network (intranet, CLETS, etc.) —
  the app shows a red **NETWORK RESTRICTED** badge so officers know it
  won't work from a public connection.

Officers can also add their own local bookmarks in the app; those stay
on their device and are not part of this repo.

---

## `california_statutes.json` — Find Code picker

```json
{
  "schema": "warrantflow.statutes.v1",
  "description": "...",
  "records": [
    {
      "code_type": "PC",
      "code": "459",
      "citation": "PC 459",
      "title": "Burglary",
      "keywords": ["burglary"]
    }
  ]
}
```

| field | purpose |
|---|---|
| `code_type` | Short abbreviation: `PC`, `VC`, `HS`, `BPC`, `WIC`, etc. The app expands this to the full code name on the warrant (e.g. `PC` → `Penal Code`). |
| `code` | The section number, e.g. `459` or `23152(a)`. |
| `citation` | Optional short citation, e.g. `PC 459`. The app prefers the expanded long form on output, but this string is shown in the picker as a hint. |
| `title` | Plain-English offense name (e.g. `Burglary`). |
| `keywords` | Free-text words the in-app search matches against (typing `burg` finds Burglary). |

The catalog also ships **embedded inside the .exe**, so the picker works
on first launch with no internet. The online copy here is just for
refreshes when California renumbers a code.

### Adding statutes

1. Edit `california_statutes.json`.
2. Validate (`python -m json.tool california_statutes.json`).
3. Commit and push.
4. Online users get the update on the next Find Code refresh. Officers
   who only have the embedded copy get it whenever they download a
   newer build of the .exe.

---

## `home.json` — Home dashboard banner + update notice

```json
{
  "latest_app_version": "1.0.1",
  "update_message": "Contact Cody Becker for the latest software.",
  "download_url": "https://github.com/BeckerIndustries/WarrantFlow/releases/latest",

  "announcements": [
    { "title": "...", "body": "..." }
  ],
  "supporters": [
    { "name": "Officer Smith", "note": "Beta tester" }
  ]
}
```

- `latest_app_version` — when newer than the running app's version, the
  home dashboard shows a red **Update Available** banner.
- `update_message` — optional explanatory line under the banner.
- `download_url` — optional; the banner adds a **Download** button that
  opens this URL. Point it at the Releases page.

Bump `latest_app_version` whenever you publish a new release. Officers on
older versions see the banner the next time they're online (after the
~5-min CDN delay).

---

## Validating before push

Quick sanity check the whole repo:

```sh
python -m json.tool catalog.json > /dev/null
python -m json.tool bookmarks.json > /dev/null
python -m json.tool california_statutes.json > /dev/null
python -m json.tool home.json > /dev/null
```

If all four exit silently, the JSON is well-formed.
