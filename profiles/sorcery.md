# Profile: Sorcery: Contested Realm

Status: draft, set codes and id namespaces need pinning against Curiosa

| Item     | Value |
|----------|-------|
| `game`   | `sorcery` |
| Database | [Curiosa](https://curiosa.io/) (official) |

## Fields

- `name`: the card name as in Curiosa.
- `set`: lowercase set identifier, e.g. `"alpha"`, `"beta"`,
  `"arthurian_legends"`. Exact tokens to be pinned against Curiosa.
- `number`: collector number within the set, where the set has them.

## `id` namespaces

| Namespace    | Value |
|--------------|-------|
| `curiosa`    | Curiosa card identifier |
| `cardmarket` | Cardmarket product id |
| `tcgplayer`  | TCGplayer product id |

## `finish` values

`nonfoil` (default), `foil`.

## Open

Sorcery distinguishes product variants (e.g. Kickstarter/preconstructed
printings) and card rarity (Ordinary, Exceptional, Elite, Unique) that may or
may not be printing-identifying. Input from Sorcery collectors on what
uniquely identifies a printing is needed before this profile leaves draft.
