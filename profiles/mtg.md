# Profile: Magic: The Gathering

Status: draft

| Item     | Value |
|----------|-------|
| `game`   | `mtg` |
| Database | [Scryfall](https://scryfall.com/docs/api) |

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
