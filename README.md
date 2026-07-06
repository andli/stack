# .stack

A standardized file format for importing and exporting lists of trading cards.

**Status: RFC.** This is a proposal. Nothing here is final. Feedback is
requested via [issues](../../issues).

## The problem

Every collection site, deck builder and marketplace exports card lists in its
own CSV or text dialect. Column names differ, data types are loose, and
importing a list from one site into another is a guessing game that regularly
fails or silently drops data.

## The proposal

One small JSON format with fixed field names and strict types, validated by a
published [JSON Schema](schema/stack.schema.json). If a file is valid, an
import will succeed. That is the whole promise, and the schema is what makes
it enforceable.

```json
{
  "stack": "1.0",
  "game": "mtg",
  "cards": [
    { "qty": 4, "name": "Lightning Bolt", "set": "m21", "number": "234", "finish": "foil" }
  ]
}
```

Only `qty` and `name` are required per card. Everything else (set, collector
number, external ids, finish, language, condition, grouping) is optional, so
the format covers everything from a scribbled-down decklist to a full
collection export with Scryfall and Cardmarket ids.

Read the spec: [SPEC.md](SPEC.md)

## Design goals

- **Guaranteed import.** Validity is machine-checkable. Importers must accept
  every valid file and must ignore what they do not understand.
- **Human-readable first.** Card name and set are the identity. External ids
  (Scryfall, Cardmarket, ...) are an optional reference for exact printing
  resolution, never a requirement. You can read and write a `.stack` file in
  a text editor.
- **Trivial to adopt.** A site that already exports CSV can emit `.stack` with
  a few dozen lines of code. No library required beyond a JSON serializer.
- **Game-agnostic core, per-game profiles.** The envelope and card fields work
  for any TCG. [Profiles](profiles/) pin what names, set codes and id
  namespaces mean per game: [MTG](profiles/mtg.md),
  [Pokemon](profiles/pokemon.md), [Yu-Gi-Oh!](profiles/yugioh.md).
- **Minimal.** Two required fields per card. Extensions are additive.

## Why JSON and not CSV?

The industry exports CSV today, and CSV is exactly why imports fail: quoting,
encodings, Excel mangling, semicolon locales, and no types. A "strict CSV"
spec cannot be enforced, because most CSVs are round-tripped through tools
that break strictness.

Adoption is not hurt by JSON, because the people who adopt this format are
developers at sites and app makers, and every one of their stacks already
speaks JSON. End users never open the file; they download from one site and
upload to another. For users who want a spreadsheet, sites can keep their CSV
exports alongside `.stack`.

## Non-goals

- Not a card database. The format carries references, not card data
  (no rules text, no images).
- Not a pricing format. Market data is out of scope for v1 (see open
  questions).
- Not a game rules format. `group` is a plain label, the spec does not define
  deck legality or zone semantics.

## Open questions

Input is specifically requested on these:

1. Prices. This will come up. Collections track acquisition price,
   marketplaces export listing prices, and users will ask for it. But price
   data drags in currency codes, price kinds (paid, ask, market, trend),
   timestamps (a price without a date is noise) and a maintenance burden that
   card identity does not have. Should v1 define a price field, reserve a
   shape for v1.x, or declare it permanently out of scope?
2. Condition scale: choosing a standard here is genuinely hard. There is no
   industry-wide grading standard, scales differ per marketplace (7 grades on
   Cardmarket, 5 on TCGplayer), grading is subjective, and every mapping
   between scales is lossy. v1 picks the 7-grade scale since a finer scale can
   represent a coarser one, but this is the most contested field in the spec.
   Should the spec define the mapping from the 5-grade scale
   (NM/LP/MP/HP/DMG), leave it to exporters, or drop `condition` entirely?
3. Yu-Gi-Oh! rarity as printing identity, see [the profile](profiles/yugioh.md).
4. Should there be a registry of `game` tokens and `id` namespaces in this
   repo, so two sites never mint the same name for different things?
5. A non-normative CSV column mapping for spreadsheet workflows: useful bridge
   or scope creep?
6. Media type registration, e.g. `application/x.stack+json`.

## Validating a file

Any JSON Schema validator works, e.g.:

```sh
npx ajv-cli validate -s schema/stack.schema.json -d examples/minimal.stack
```

## Examples

See [examples/](examples/): [minimal](examples/minimal.stack),
[collection with ids and conditions](examples/mtg-collection.stack),
[deck with groups](examples/mtg-deck.stack),
[another game](examples/pokemon.stack).

## License

CC0 1.0 (public domain). Anyone can implement this format for any purpose
without permission or attribution.
