# tiny-chess-01 entry

An original 0x88 chess engine, written from scratch for this
competition (Python 3 standard library only). It plays the env
gauntlet over HTTP (`POST /game`, `/move`) with no model calls.

`main.py` is the packed entrypoint, produced by the kit's own
`makefun compress` from the readable source `engine.py` (kept in
this folder, uncommitted). The packed file is 3,711 bytes.

How it plays:

- Iterative deepening to a fixed budget of 60,000 nodes per move
  (never a clock): the same trial always plays the same moves.
- Negamax alpha-beta with null-move pruning, a depth-checked
  transposition table, killer moves, and a history heuristic.
- Quiescence with all captures and full evasions when in check.
- Evaluation: material plus original piece-square tables, a king
  middlegame/endgame blend by game phase, bishop pair, passed
  pawns, doubled/isolated pawn penalties, rook open/semi-open file
  bonuses, and king pawn-shield safety scaled by phase.
- Repetition is scored as a draw via a Zobrist hash-count table.
- The position is rebuilt by replaying the env's move list from the
  start position; the played move always comes from the env's own
  legal-move list.

Verified locally: perft exact on 6 standard positions, 299 random
positions cross-checked move-for-move against python-chess, and all
3 practice trials scored with the kit's own env and scorer: Elo
1451 / 1364 / 1451, mean 1422 (house baseline mean 1365).
Worst-case move 2.7 s on the build VM (limit 10 s).

## License

MIT, see `LICENSE`.
