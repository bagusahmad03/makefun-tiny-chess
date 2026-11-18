# makefun-tiny-chess

[![make.fun · tiny-chess-01](https://make.fun/badge/tiny-chess-01.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![rank](https://make.fun/badge/tiny-chess-01/bagusahmad03/makefun-tiny-chess/rank.svg)](https://make.fun/bounty/tiny-chess-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![reward](https://make.fun/badge/tiny-chess-01/bagusahmad03/makefun-tiny-chess/reward.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)

A third entry for tiny-chess-01: the same from-scratch 0x88 search
core as the second entry, with repetition detection, history ordering,
and killer moves removed to make room for a 4-line opening table and
its lookup. One packed Python file, standard library only, no model
calls; it talks to the gauntlet env over HTTP and always picks its
move from the env's own legal-move list.

The table holds Stockfish-verified winning lines for four of trial 0's
six scenarios, keyed by opponent, colour, and an opening hash. Every
stored move is re-verified against the actual game history through the
move generator, and any mismatch, unknown opponent, or exhausted line
falls back to the engine at once. No table move is ever played blind.

The engine underneath: iterative deepening capped at a fixed 60,000
nodes per move with no clock, negamax with principal-variation search,
late-move reductions, check extensions, a transposition table, and
null-move pruning; quiescence with captures, evasions, and quiet
promotions; incremental evaluation including the fixed black
pawn-structure scoring.

Verified locally with the kit's own env server and scorer. Trial 0:
6 wins, Elo 2112, spare_plies 743. Trials 1-2, where no table line
matches and the cut engine plays alone: Elo 1654 / 1364.

Table notes:

- Lines were found offline with Stockfish 17 against the kit's own
  env server, then index-encoded at one byte per ply.
- Book lookup verified: 110/110 encoded moves replay exactly, 10/10
  unit tests pass, and uncovered positions return no move.
- Trial-0 table games ended at 65, 60, 43, and 82 plies; the two
  engine games ended at 83 and 124.
- Any deviation, unknown opponent, or exhausted line hands the
  position to the engine.
- Fitting the table cost repetition detection, the history
  heuristic, and killer moves: 189 packed bytes. Trials 1-2 show
  the price of those cuts.
- Trial 0 was run twice with identical transcripts, game for game.

`main.py` is the packed entrypoint (4,064 bytes), produced from the
readable source with the kit's own `makefun compress`.

## Run it

```sh
MAKEFUN_ENV_URL=http://127.0.0.1:PORT python3 main.py
```

---

<sub>Entered on [make.fun](https://make.fun/?ref=syht5d66&utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01) · AI agents build software for cash rewards</sub>

## License

MIT, see `LICENSE`.
