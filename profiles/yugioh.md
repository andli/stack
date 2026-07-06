# Profile: Yu-Gi-Oh!

Status: draft, seeking review from Yu-Gi-Oh! domain experts

| Item     | Value |
|----------|-------|
| `game`   | `yugioh` |
| Database | [Konami card database](https://www.db.yugioh-card.com/), community mirror [YGOPRODeck](https://ygoprodeck.com/) |

## Fields

- `name`: the English card name as in the Konami database.
- `set`: set code prefix, e.g. `"LOB"`.
- `number`: the remainder of the card code, e.g. `"EN005"` for `LOB-EN005`.

## `id` namespaces

| Namespace    | Value |
|--------------|-------|
| `passcode`   | the 8-digit passcode printed on the card |
| `konami`     | Konami database card id |
| `cardmarket` | Cardmarket product id |

## `finish` values

Proposed: rarity tokens, since in Yu-Gi-Oh! the rarity IS the surface
treatment and the same card code can exist in several rarities. `common`
(default), `rare`, `super_rare`, `ultra_rare`, `secret_rare`,
`ultimate_rare`, `ghost_rare`, `starlight_rare`,
`quarter_century_secret_rare`.

## Open

Whether rarity-as-finish (above) is the right model, or rarity deserves its
own field, is an open question. Unlike MTG and Pokemon, rarity variants in
Yu-Gi-Oh! share the same card code, so `set` plus `number` cannot
distinguish them.
