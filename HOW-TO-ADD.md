# How to add knowledge to myKnowledge

Quick guide for the two things you'll do most often. The full format rules live in [OKF.md](./OKF.md); every unit (e.g. [butyrate](./health/components/butyrate/manifest.json)) is a working example.

## 1. Add a file to an existing topic

Example: adding a second source PDF to butyrate (`health/components/butyrate/`).

1. Copy the file into the unit's `files/` folder.
2. Register it in `manifest.json` — in **both** `files[]` and `sources[]` (a file without a source entry is orphaned).
3. Link it from `notes.md` under Sources.
4. Bump `version` (`1.0.0` → `1.1.0`) and set `updated` to today's date (`YYYY-MM-DD`).
5. Commit.

```json
"sources": [
  {
    "type": "article",
    "author": "Mercola",
    "title": "Understanding butyrate",
    "date": "2026-06",
    "file": "files/understanding-butyrate-pdf.pdf"
  },
  {
    "type": "study",
    "author": "New Author",
    "title": "New paper on butyrate",
    "date": "2026-11-20",
    "file": "files/new-paper.pdf"
  }
],
"files": [
  { "path": "files/understanding-butyrate-pdf.pdf", "kind": "source_pdf" },
  { "path": "files/new-paper.pdf", "kind": "source_pdf" }
]
```

## 2. Add a brand-new topic (new unit)

Example: adding creatine as a new component.

1. Create the folder under the right domain + type: `health/components/creatine/` with a `files/` subfolder, and copy the source files in. Slug = lowercase kebab-case, ASCII only — no spaces, no `&`.
2. Write `manifest.json` — copy an existing manifest (e.g. butyrate's) as a template and change:
   - `id` → `health.component.creatine` (must equal `<domain>.<type>.<slug>`)
   - `title`, `summary`, `tags`
   - `created` / `updated` → today
   - `version` → `"1.0.0"`
   - `details` → follow the type conventions in [OKF.md](./OKF.md#conventions-for-details-per-type) (for a component: `what_it_is`, `mechanisms`, `benefits`, `side_effects_and_cautions`, `how_to_use`, `good_for`)
   - `sources[]` and `files[]` → point at the files you copied
3. Write `notes.md` — the human-readable deep dive, with relative links like `[Mercola, Jun 2026](files/understanding-butyrate-pdf.pdf)`.
4. Register the unit in the domain index `health/_index.json` — append to `units[]` and bump the index `updated`:

```json
{
  "id": "health.component.creatine",
  "type": "component",
  "title": "Creatine",
  "path": "components/creatine/",
  "status": "active",
  "updated": "2026-11-21"
}
```

5. Commit.

## Tips

- Set `status: "draft"` in the manifest and index while a unit is still being researched; flip to `"active"` when done.
- Use `relations` to connect units, e.g. butyrate and methylene blue both touch mitochondria:

```json
"relations": [
  { "id": "health.component.methylene-blue", "type": "related_to" }
]
```

- Validate after editing: all JSON must parse, and every path in `files[]`, `sources[].file`, and every `files/...` link in `notes.md` must exist.
