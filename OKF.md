# Open Knowledge Format (OKF) — v0.1

A lightweight, file-based format for organizing knowledge in a Git repository. Every piece of knowledge is an **atomic unit** in its own folder, described by a machine-readable `manifest.json`, explained in human-readable `notes.md`, and backed by its original `files/`.

## Principles

1. **Atomic units** — one folder per component, protocol, condition, etc. A unit can be moved, copied, or deleted without breaking anything else.
2. **Self-contained** — manifest + notes + files travel together inside the unit folder.
3. **Machine-readable + human-readable** — `manifest.json` for queries and agents, `notes.md` for reading.
4. **Domain-first** — the repo root is split by domain (`health/`, later `tech/`, `finance/`...), so each domain scales independently.
5. **Sources always cited** — every unit lists its sources; raw source files live in `files/`.
6. **Versioned** — Git provides history; each unit also carries its own `version` and `updated`.

## Layout

```
myKnowledge/
├── OKF.md                    # this specification
├── README.md                 # repo entry point, index of domains
└── <domain>/                 # e.g. health/
    ├── _index.json           # registry of all units in the domain
    └── <type>/               # core types, or domain-specific types (see manifest `type` field)
        └── <unit-slug>/      # lowercase-kebab-case, ASCII only
            ├── manifest.json # machine-readable metadata (required)
            ├── notes.md      # human-readable deep dive (required)
            └── files/        # original sources (pdf, images, ...)
```

## manifest.json — common schema (v0.1)

| Field         | Required | Description |
| ------------- | -------- | ----------- |
| `okf_version` | yes      | Format version, e.g. `"0.1"` |
| `id`          | yes      | Globally unique: `<domain>.<type>.<slug>`, e.g. `health.component.methylene-blue` |
| `type`        | yes      | Core types: `component`, `protocol`, `condition`, `practice`, `concept`, `tool`, `reference` — a domain may also define its own types (e.g. botany: `ecology`, `paysage`, `horticulture`) |
| `title`       | yes      | Human-readable title |
| `status`      | yes      | `draft` \| `active` \| `archived` |
| `version`     | yes      | Semver of the unit's content (`"1.0.0"`) |
| `created`     | yes      | ISO date `YYYY-MM-DD` |
| `updated`     | yes      | ISO date `YYYY-MM-DD` |
| `summary`     | yes      | 1–3 sentence abstract |
| `tags`        | no       | Flat list of lowercase keywords |
| `details`     | no       | Type-specific structured fields (free-form object, see conventions below) |
| `sources`     | yes      | List of `{type, author, title, date, url?, file?, accessed?}` |
| `relations`   | no       | List of `{id, type}` links to other units (`synergizes_with`, `used_in`, `contraindicated_with`, ...) |
| `files`       | no       | List of `{path, kind, ...}` — paths relative to the unit folder |

## notes.md

Free-form markdown deep dive: what it is, mechanisms, benefits, usage, cautions, open questions. Reference source files as relative links, e.g. `[Mercola, Oct 2024](files/methylene-blue-benefits-side-effects-pdf.pdf)`.

## _index.json (one per domain)

```json
{
  "okf_version": "0.1",
  "domain": "health",
  "title": "Health",
  "updated": "2026-11-21",
  "units": [
    { "id": "health.component.methylene-blue", "type": "component", "title": "Methylene blue", "path": "components/methylene-blue/", "status": "active", "updated": "2026-11-21" }
  ]
}
```

## Naming rules

- Folders and slugs: lowercase kebab-case, ASCII only — no spaces, no `&` (e.g. `bio-protocolo-cancer`, not `integrative & metabolic oncology`).
- The `id` mirrors the path: `health/protocols/bio-protocolo-cancer` → `health.protocol.bio-protocolo-cancer`.

## Conventions for `details` per type

- **component**: `what_it_is`, `mechanisms[]`, `benefits{}`, `side_effects_and_cautions[]`, `how_to_use{}`, `good_for[]`
- **protocol**: `classification` (category, subcategory, keywords, collections), `sections[]`, `reliability{}`
- Extend with new keys as new types appear; keep the common schema stable.

## Adding a unit (checklist)

1. Create `<domain>/<type>/<unit-slug>/` and copy original files into `files/`.
2. Write `manifest.json` (valid JSON, unique `id`).
3. Write `notes.md`.
4. Add the unit to `<domain>/_index.json`.
5. Commit.
