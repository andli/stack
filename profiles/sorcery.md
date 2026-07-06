# Profile: Sorcery: Contested Realm

Status: draft

| Item     | Value |
|----------|-------|
| `game`   | `sorcery` |
| Database | [Curiosa](https://curiosa.io/) (official), API at `api.sorcerytcg.com` |

## Fields

- `name`: the card name as in Curiosa, e.g. `"Apprentice Wizard"`.
- `set`: lowercase set code as used in Curiosa variant slugs, e.g. `"alp"`
  (Alpha), `"bet"` (Beta).
- `number`: not used. Sorcery cards have no collector numbers; a printing is
  identified by set, product and finish. For exact printing identity use the
  `curiosa` id namespace.

## `id` namespaces

| Namespace    | Value |
|--------------|-------|
| `curiosa`    | Curiosa variant slug, e.g. `"alp-apprentice_wizard-b-s"` (encodes set, card, product, finish) |
| `cardmarket` | Cardmarket product id |
| `tcgplayer`  | TCGplayer product id |

## `finish` values

`standard` (default), `foil`, matching Curiosa's finish enumeration.

## Open

The same card, set and finish can exist in several products (Booster,
Welcome Kit, Preconstructed). Without collector numbers, the human-readable
fields cannot distinguish those printings; only the `curiosa` slug can.
Whether product deserves a spec-level or profile-level field is an open
question for Sorcery collectors.
