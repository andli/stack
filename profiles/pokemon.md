# Profile: Pokemon TCG

Status: draft, seeking review from Pokemon TCG domain experts

| Item     | Value |
|----------|-------|
| `game`   | `pokemon` |
| Database | [Pokemon TCG API](https://pokemontcg.io/) |

## Fields

- `name`: the card name as in the Pokemon TCG API.
- `set`: Pokemon TCG API set id, e.g. `"sv1"`, `"base1"`.
- `number`: card number within the set, e.g. `"25"`.

## `id` namespaces

| Namespace      | Value |
|----------------|-------|
| `pokemontcgio` | Pokemon TCG API card id, e.g. `"sv1-25"` |
| `cardmarket`   | Cardmarket product id |
| `tcgplayer`    | TCGplayer product id |

## `finish` values

`nonfoil` (default), `holo`, `reverse_holo`.

Rarity-based variants (rainbow rare, gold, full art, illustration rare, ...)
are not finish tokens: they have their own card numbers in the set, so they
are identified by `set` plus `number`. `finish` only carries the surface
treatment of one and the same card number.

## Open

Edition stamps (1st Edition, Shadowless, Unlimited) are orthogonal to finish;
a 1st Edition Holofoil is both at once, so `finish` cannot carry them.
Whether editions need their own field is an open question.
