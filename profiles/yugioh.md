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

## Open

Rarity is part of printing identity in Yu-Gi-Oh! (the same card code can exist
in several rarities). Whether rarity belongs in `finish`, in a new field, or in
this profile only, is an open question.
