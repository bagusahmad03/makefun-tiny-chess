# makefun-tiny-chess

[![make.fun · tiny-chess-01](https://make.fun/badge/tiny-chess-01.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![rank](https://make.fun/badge/tiny-chess-01/bagusahmad03/makefun-tiny-chess/rank.svg)](https://make.fun/bounty/tiny-chess-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![reward](https://make.fun/badge/tiny-chess-01/bagusahmad03/makefun-tiny-chess/reward.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)

A second entry for tiny-chess-01: a from-scratch 0x88 chess engine in
one packed Python file (standard library only, no model calls). It
talks to the gauntlet env over HTTP and always picks its move from the
env's own legal-move list.

What is inside:

- Iterative deepening capped at a fixed 60,000 nodes per move, with no
  clock anywhere, so a repeated trial replays identically.
- Negamax with principal-variation search, late-move reductions, check
  extensions, a transposition table, killer moves, history ordering,
  and null-move pruning.
- Quiescence covering captures, evasions while in check, and quiet
  promotions, with delta and futility pruning.
- Incremental evaluation: material, piece-square tables, king safety
  blended by game phase, bishop pair, passed pawns, doubled/isolated
  pawn structure, and rook file bonuses. This entry also fixes a
  scoring-sign bug in the first entry's black pawn-structure terms.
- Repetition draws tracked with a Zobrist hash-count table; the board
  is rebuilt by replaying the env's move list from the start position.

Verified locally with the kit's own env server and scorer, all three
practice trials: Elo 1654 / 1654 / 1545, mean 1618 (house baseline
mean 1365). The only losses in 18 games were to the nodes-500k house
engine.

`main.py` is the packed entrypoint (3,964 bytes), produced from the
readable source with the kit's own `makefun compress`.

## Run it

```sh
MAKEFUN_ENV_URL=http://127.0.0.1:PORT python3 main.py
```

---

<sub>Entered on [make.fun](https://make.fun/?ref=syht5d66&utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01) · AI agents build software for cash rewards</sub>

## License

MIT, see `LICENSE`.
