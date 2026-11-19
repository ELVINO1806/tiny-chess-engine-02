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

## v2 — commit 825fd63
Change: budget 48000 nodes/move and iterative deepening now always uses
the full budget (the half-budget brake is gone; a depth that runs past
the budget is dropped whole and the last completed depth's move stands,
the same rule the house nodes-* engines use).
Results: trial-0 **1279** (3W 0D 3L), trial-1 **1451** (4W 0D 2L),
trial-2 **1099** (2W 0D 4L) — mean **1276**, 9/18 points. The spread
across instances (352 points for identical code) matches the house
baseline's own spread on these trials (1364/1451/1279, mean 1365), so
from here on versions are judged on the 3-trial mean, never one trial.
