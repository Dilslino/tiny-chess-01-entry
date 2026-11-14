# tiny-chess-01

A compact chess engine in a single Python file under 4,096 bytes.

## How it is built

`main.py` carries the engine as an LZMA payload (FORMAT_ALONE, preset 9,
base85-encoded) plus a three-line unpacker. The unpacker decompresses the
payload and runs it with `__name__` set to `__main__`, so the engine's own
driver starts exactly as if the source had been run directly.

The payload is produced in-session by `pack.py` (kept out of this repo):
it reads the engine source, compresses it, and injects it into the stub
above. No engine line is copied by hand into this repository.

## The engine

Negamax alpha-beta search with null-move pruning, quiescence over
captures and promotions, a transposition table, MVV-LVA move ordering,
and piece-square evaluation. It searches a fixed node budget per move
(50,000 nodes) and never reads a clock, so a game replays move for move.

It plays through the competition env: it reads the game from
`MAKEFUN_ENV_URL`, posts its moves, and exits when every game is over.

## Verification

The repacked file is graded locally with the kit before submitting:
`node cli/makefun.mjs grade-local --kit . --bot . --driver process`.
The prior build of this engine measured Elo 1253 on the practice set;
the repack is byte-identical after decompression, so its strength is
unchanged. The local grade is re-run on the repacked file to confirm.
