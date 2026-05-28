# WarrantFlow Shared Content

This repository hosts the public catalogs the WarrantFlow desktop app
downloads and caches:

- **`catalog.json`** — reusable clauses for the **Language Library** (places,
  items, affiant expertise, probable cause).
- **`bookmarks.json`** — shared web links for the **Bookmarks** page.
- **`home.json`** — supporter shoutouts, news/announcements, and the
  latest-version info for the **Home** dashboard.

### home.json — update banner fields

```json
{
  "latest_app_version": "1.0.1",   // when newer than the running app, a red
                                    //   "Update Available" banner shows
  "update_message": "Contact Cody Becker for the latest software.",  // optional
  "download_url": "https://example.com/WarrantFlow.zip"              // optional; shows a Download button
}
```

Bump `latest_app_version` whenever you ship a new build. Officers on an older
version see the banner the next time they're online (after the ~5-min CDN
delay). Leave `download_url` empty to show the message with no button.

Officers can also add their own local bookmarks in the app; those stay on
their device and are not part of this repo.

## bookmarks.json schema

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
      "category": "legal",      // records | legal | mapping | agency | training | other
      "tags": ["search", "keywords"],
      "network_restricted": false  // optional; true shows a red "NETWORK RESTRICTED" badge
    }
  ]
}
```

Only `http://` and `https://` URLs will open from the app.

`network_restricted` is optional and defaults to `false`. Set it to `true`
for links that only resolve on the agency network (intranet, CLETS, etc.) —
the app shows a red **NETWORK RESTRICTED** badge so officers know it won't
work from a public connection.

---

## The catalog

`catalog.json` is the single file the app reads. Schema:

```json
{
  "version": 1,                 // bump when you change the catalog
  "updated": "2026-05-27",      // yyyy-MM-dd
  "snippets": [
    {
      "id": "unique-stable-id",       // never reuse; lowercase-with-hyphens
      "category": "item",             // see categories below
      "title": "Human-readable title",
      "body": "The clause text. Use [BRACKETS] for fill-in blanks.",
      "tags": ["search", "keywords"]  // used by the in-app search box
    }
  ]
}
```

### Categories (must be exactly one of these)

| value | meaning |
|---|---|
| `place` | Places to be searched (residence, vehicle, account) |
| `item` | Items / property / records to be seized |
| `expertise` | Affiant-expertise boilerplate paragraphs |
| `probable_cause_fragment` | Reusable probable-cause language |
| `other` | Anything else |

## How to add or edit a clause

1. Edit `catalog.json`.
2. Keep `id` values stable and unique. Add new IDs for new clauses; don't
   recycle an old ID for different content.
3. Bump the top-level `version` and update `updated`.
4. Validate the JSON (any JSON linter, or `python -m json.tool catalog.json`).
5. Commit and push. The app picks up changes the next time a user clicks
   **Refresh** in Warrant Templates.

## Notes

- The catalog is **public and unauthenticated** — don't put anything
  sensitive or agency-confidential here. These are generic, reusable clauses.
- `[BRACKETED]` placeholders are deliberately left for the officer to fill in
  after inserting the clause into a warrant.
