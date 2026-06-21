# try-example

A [Claude Code](https://claude.com/claude-code) skill that teaches one concept by
**building the smallest runnable project that demonstrates it — one layer at a time**,
stopping for your explicit approval before each new layer.

You learn by watching the example grow, not by being handed the finished thing.

## How it works

```
/try-example cosine similarity for course recs
```

1. **Mental model first.** It states the single idea the concept rests on, then asks
   you to restate it. Nothing gets built until you do.
2. **One layer per turn.** It writes the smallest meaningful increment, *runs it*, and
   shows the real output.
3. **Approval gate.** It explains the layer, asks a comprehension check, then stops.
   The next layer doesn't appear until you say "next".
4. **Done when it's convincing.** It stops at a decent example — not a product. No
   frameworks, no polish the concept didn't need.

Stdlib-first, zero new dependencies unless the concept truly needs one, fewest files,
each concept in its own isolated folder.

## Install

```sh
git clone https://github.com/jerryzhou196/try-example.git ~/Github/try-example
ln -s ~/Github/try-example ~/.claude/skills/try-example
```

Restart Claude Code so it picks up the skill, then run `/try-example <concept>`.

## License

MIT
