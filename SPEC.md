# The .stack format, version 1.0 (draft)

A `.stack` file is a machine-readable list of trading cards. It is designed for
import and export between collection sites, deck builders and marketplaces.
Field names and data types are fixed, so a valid file always imports.

The key words MUST, MUST NOT, SHOULD and MAY are to be interpreted as described
in [RFC 2119](https://www.rfc-editor.org/rfc/rfc2119).

## File

- A `.stack` file is a single JSON document (RFC 8259), UTF-8, no BOM.
- The file extension SHOULD be `.stack`.
- Validity is defined by [schema/stack.schema.json](schema/stack.schema.json).
  A file is a valid `.stack` file if and only if it validates against the
  schema for its `stack` version.

## Envelope

| Field         | Type   | Required | Description |
|---------------|--------|----------|-------------|
| `stack`       | string | yes      | Spec version, e.g. `"1.0"`. See Versioning. |
| `game`        | string | yes      | Lowercase game identifier, e.g. `"mtg"`, `"pokemon"`, `"yugioh"`, `"lorcana"`. |
| `cards`       | array  | yes      | List of card entries. MAY be empty. |
| `name`        | string | no       | Human-readable name of the list. |
| `description` | string | no       | Free-text description. |
| `source`      | string | no       | Name of the exporting app or site. |
| `created`     | string | no       | Export timestamp, ISO 8601 date-time. |

## Card entry

| Field       | Type    | Required | Description |
|-------------|---------|----------|-------------|
| `qty`       | integer | yes      | Number of copies. Minimum 1. |
| `name`      | string  | yes      | Card name as printed or as named in the game's canonical database. |
| `set`       | string  | no       | Set or expansion code as used by the game's canonical database, e.g. `"m21"`. Authoritative for resolution. |
| `set_name`  | string  | no       | Human-readable set name, e.g. `"Core Set 2021"`. Display and cross-check companion to `set`; resolution fallback when `set` is absent. |
| `number`    | string  | no       | Collector number within the set. String, because numbers like `"234a"` exist. |
| `id`        | object  | no       | External identifiers, as supplementary machine hints only. Keys are lowercase namespace names, values are strings. Example: `{"scryfall": "f2ab…", "cardmarket": "265535"}`. |
| `finish`    | string  | no       | Lowercase token. Recommended values are listed per game profile, e.g. `foil`. Absent means the default finish. |
| `lang`      | string  | no       | ISO 639-1 language code, lowercase, e.g. `"en"`, `"ja"`. Absent means `"en"`. |
| `condition` | string  | no       | One of `mint`, `near_mint`, `excellent`, `good`, `light_played`, `played`, `poor`. Absent means unspecified. |
| `group`     | string  | no       | Free-text grouping label, e.g. `"main"`, `"sideboard"`, `"Binder A"`. Absent means ungrouped. |

The same card MAY appear in multiple entries, e.g. to express different
conditions or finishes of the same printing.

Note on `condition`: there is no industry-wide grading standard. Marketplaces
use different scales (Cardmarket uses 7 grades, TCGplayer uses 5) and grading
is subjective. This spec picks the 7-grade scale as the wire format because a
finer scale can represent a coarser one, but any mapping between scales is
lossy and contested. Exporters mapping from another scale SHOULD map
conservatively (downward). This choice is explicitly open for review.

## Conformance

A `.stack` file is human-readable first: every entry MUST be identifiable
from `name` (and `set`/`number` when present) alone. External ids are an
optional reference, never a substitute.

An exporter:

- MUST produce files that validate against the schema.
- SHOULD include `set`, `set_name` and `number` when the printing is known.
  Name-only entries are intended for hand-authored lists.
- MAY add `id` values as an optional machine-readable reference.
- MAY add fields not defined here. Custom fields MUST NOT change the meaning
  of defined fields.

An importer:

- MUST accept every valid file. A valid file MUST NOT be rejected as a whole
  because of values the importer cannot use.
- MUST ignore unknown fields.
- MUST NOT require any optional field.
- MAY drop or flag individual entries it cannot resolve to a card, but MUST
  still import the entries it can resolve.

## Card resolution

Importers SHOULD resolve each entry to a card in this order:

1. An `id` value in a namespace the importer understands (fast path).
2. `set` and `number` (and `name` as a cross-check). If `set` is absent,
   `set_name` MAY be used in its place.
3. `name` alone. The importer MAY pick any printing.

If an `id` contradicts the human-readable fields, the human-readable fields
are authoritative and the importer SHOULD flag the entry.

What `name`, `set`, `number` and the `id` namespaces refer to is pinned per
game by its profile. Matching SHOULD be case-insensitive.

## Game profiles

The core format is game-agnostic. A profile pins, for one game:

- the `game` identifier token,
- the canonical database that `name`, `set` and `number` refer to,
- the recognized `id` namespaces,
- the recommended `finish` values.

A profile MAY make its database normative: exporters claiming conformance
for that game MUST then use the database's names, codes and tokens for the
fields the profile pins. This makes files checkable against the actual card
catalog, not just against the schema.

Profiles live in [profiles/](profiles/) and are non-breaking additions.
A game without a profile MAY still use the format; entries then resolve by
`name` on a best-effort basis.

## Versioning

The `stack` field carries the spec version as `"major.minor"`.

- Minor versions are additive only: new optional fields, new recommended
  values. A 1.x importer MUST accept any 1.y file (unknown fields are ignored,
  see Conformance).
- Breaking changes require a new major version.
