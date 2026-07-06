# Profile: Magic: The Gathering

Status: draft

| Item     | Value |
|----------|-------|
| `game`   | `mtg` |
| Database | [Scryfall](https://scryfall.com/docs/api) (normative) |

## Conformance

Scryfall is normative for this profile, not just recommended. An exporter
emitting `game: "mtg"`:

- MUST use Scryfall set codes for `set` and Scryfall collector numbers for
  `number`.
- MUST use the Scryfall card name for `name` whenever `set` or `number` is
  present. Name-only entries (hand-authored lists) SHOULD use Scryfall names.
- MUST use only the `finish` tokens listed below.

Scryfall publishes free [bulk data](https://scryfall.com/docs/api/bulk-data),
so conformance is machine-checkable: a validator can verify every entry
against the actual card catalog, not just against the schema.

## Fields

- `name`: the Scryfall card name. For multi-face cards, exporters SHOULD use
  the full name with `" // "` (e.g. `"Fire // Ice"`). Importers SHOULD also
  match on the front-face name alone.
- `set`: Scryfall set code, lowercase, e.g. `"m21"`.
- `number`: Scryfall collector number, e.g. `"234"`, `"234a"`.

## `id` namespaces

| Namespace    | Value |
|--------------|-------|
| `scryfall`   | Scryfall card id (UUID), identifies the exact printing |
| `oracle`     | Scryfall oracle id (UUID), identifies the card across printings |
| `mtgjson`    | MTGJSON card UUID |
| `cardmarket` | Cardmarket product id |
| `tcgplayer`  | TCGplayer product id |
| `arena`      | MTG Arena card id |
| `mtgo`       | Magic Online catalog id |

## `finish` values

`nonfoil` (default), `foil`, `etched`.

These match Scryfall's finish enumeration. Special foil treatments (surge,
galaxy, rainbow, textured, halo, confetti, oil slick, gilded, ...) are
deliberately not finish tokens: Scryfall gives those printings their own
collector numbers, so they are identified by `set` plus `number` and carry
`finish: "foil"`. This keeps the enumeration closed instead of chasing every
new treatment Wizards invents.
