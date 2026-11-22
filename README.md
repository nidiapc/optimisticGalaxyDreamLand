# myKnowledge

Personal knowledge base organized with the **Open Knowledge Format (OKF)** — see [OKF.md](./OKF.md) for the full spec (v0.1).

## Domains

| Domain | Index | Contents |
| ------ | ----- | -------- |
| Health | [health/_index.json](./health/_index.json) | 2 components, 1 protocol |
| Botany | [botany/_index.json](./botany/_index.json) | 2 horticulture units (fruits, protocols); ecology & paysage empty for now |

## Structure

```
myKnowledge/
├── OKF.md                 # OKF v0.1 specification
├── HOW-TO-ADD.md          # how to add a new topic or file
├── README.md              # this file
└── <domain>/              # e.g. health/
    ├── _index.json        # registry of all units in the domain
    └── <type>/<unit>/     # manifest.json + notes.md + files/
```

## Units

### Health

- **Components** — [methylene blue](./health/components/methylene-blue/notes.md), [butyrate](./health/components/butyrate/notes.md)
- **Protocols** — [BioProtocolo: integrative & metabolic oncology](./health/protocols/bio-protocolo-cancer/notes.md)

### Botany

- **Horticulture: fruits** — [lemon seed germination](./botany/horticulture/fruits/notes.md)
- **Horticulture: protocols** — [homemade fertilizers: common mistakes](./botany/horticulture/protocols/notes.md)

## Adding knowledge

See [HOW-TO-ADD.md](./HOW-TO-ADD.md) for step-by-step instructions (add a file to an existing topic, or create a new unit), and the checklist in [OKF.md](./OKF.md#adding-a-unit-checklist).
