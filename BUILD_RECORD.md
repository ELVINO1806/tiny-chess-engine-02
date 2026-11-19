# Build record — Tiny Chess Engine 02 (make.fun tiny-chess-01)

Measurement method: the kit's grade-local process driver runs node22 bots
only (Python runs only inside Docker, which is unavailable here), so each
version is measured by playing the kit's own env server
(program/env/server.mjs) against the shipped main.py and scoring the
exported state with the kit's own scorer (program/scorer/score.mjs) on
practice trial-0 (6 games: depth-2 W, depth-3 B, depth-4 W, nodes-100k B,
nodes-250k W, nodes-500k B; anchors 855/1073/1199/1392/1497/1596).
All engines are node-capped, so ratings are hardware-independent except
for the 10 s/move wall. Perft suite passed before v1 was committed
(startpos, Kiwipete, en-passant-pin and promotion positions).

## v1 — commit 647dfd6
Design: 0x88 board, negamax alpha-beta with PVS, transposition table,
null-move pruning (R 2/3), single-step LMR, history ordering, capture
quiescence with check evasions, check extension, tapered eval (hand-written
PSTs for P/N/B/K, king endgame table, phase blend), repetition and
100-ply draw scores in search. Budget 40000 nodes/move, iterative
deepening stops starting a new depth past half the budget.
Result trial-0: **Elo 1279** — 2 won, 2 drawn, 2 lost (working 1,
spare_plies 133). Beat depth-2 (mate) and nodes-100k (mate, as Black);
drew depth-3 and depth-4 (adjudicated at 200 plies); lost to nodes-250k
and nodes-500k (mated).
