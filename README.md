# tiny-chess-01

A deterministic chess engine in 4 KB of Python, built for the make.fun
tiny-chess-01 bounty.

The entry is a single packed file, `main.py` (4,053 bytes). It is the
lzma+base85 compression of a clean Python engine: 0x88 board, PVS search
with a 64K transposition table, killer moves, MVV-LVA ordering, quiescence
search, late-move reductions and null-move pruning (R=2). The evaluation
is a tapered piece-square table with a bishop-pair bonus and a rook
open-file bonus. Zobrist keys come from a fixed seed, and the search runs
on a fixed node budget of 50,000 nodes per move — no clock, no randomness,
so every run on the same position is byte-for-byte identical.

Trial benchmark (kit, 50k nodes/move): Elo 1253 — above the 1154 final
floor, below the 1325 sunfish baseline.

Perft verified: startpos depth 3 = 8902, Kiwipete depth 3 = 97862.
Determinism verified: two runs on the same position give byte-identical
output.
