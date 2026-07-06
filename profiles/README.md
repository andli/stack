# Game profiles

A profile pins what the core fields mean for one game: which database
`name`, `set` and `number` refer to, which `id` namespaces exist, and which
`finish` tokens are valid. The core spec defines the shape; profiles define
the vocabulary. See [SPEC.md](../SPEC.md#game-profiles).

## Want a profile for game X?

1. Check the [issues](../../../issues) for an existing proposal or
   profile-wanted request for the game.
2. Copy [TEMPLATE.md](TEMPLATE.md) to `<game>.md` and fill it in. The hard
   part is almost always printing identity: what combination of
   human-readable fields uniquely identifies one physical printing? Name the
   cases where they cannot (see the Sorcery profile's Open section for an
   example) instead of papering over them.
3. Open a PR. State your relationship to the game (collector, tool author,
   site operator) and whether you are willing to own the profile, meaning
   you answer questions and review changes to it.

A profile needs a canonical, publicly accessible database to pin against.
If the game has none, that problem has to be solved first, and an issue is
the right place to discuss it.

Requirements for acceptance:

- `game` token is lowercase `[a-z0-9_]+` and not already taken.
- Every field the profile pins refers to the named database, not to a
  private catalog.
- `finish` tokens are a closed list. Variants that the database gives their
  own set or collector number are not finish tokens.
- An owner. Unowned profiles stay marked draft.
