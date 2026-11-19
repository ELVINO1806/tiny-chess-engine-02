# Build record — Tiny Chess Engine 02 (make.fun tiny-chess-01)

Method: grade-local's process driver runs node22 bots only (Python needs
Docker, unavailable here), so each version plays the kit's own env server
(program/env/server.mjs) with the shipped main.py and is scored by the
kit's own scorer on the practice trials (6 games each: depth-2 W,
depth-3 B, depth-4 W, nodes-100k B, nodes-250k W, nodes-500k B; anchors
855/1073/1199/1392/1497/1596). Engines are node-capped, so ratings are
hardware-independent apart from the 10 s/move wall. Versions are judged
on the 3-trial mean (identical code spans 350+ points across instances;
the house baseline itself rates 1364/1451/1279, mean 1365). Perft suite
(startpos, Kiwipete, en-passant-pin, promotion positions) passed before
v1 and after every search change.

## v1 — 647dfd6 — trial-0: Elo 1279 (2W 2D 2L)
0x88 board; negamax alpha-beta + PVS, TT, null-move (R 2/3), 1-ply LMR,
history ordering, capture quiescence with check evasions, check
extension, tapered PST eval (P/N/B + king MG/EG tables), repetition and
100-ply draws in search. Budget 40000, deepening stops past half budget.

## v2 — 825fd63 — 1279 / 1451 / 1099, mean 1276 (9/18)
Budget 48000, always fully used (an over-budget depth is dropped whole,
the house nodes-* rule).

## v3 — 854a6a7 — 1099 / 1279 / 1279, mean 1219 (8/18) — not kept
Budget 54000, two-tier LMR (2-ply for late moves), bishop pair restored.
Late-move pruning was tried in development and rejected pre-commit: at
depth <= 3 it corrupted horizon scores (depth-4 startpos picked 1.Na3).

## v4 — c87161c/cc80df4 — 1191 / 1191 / 1279, mean 1220 (8.5/18)
LMR back to 1-ply, budget 64000, bishop pair kept, behaviour-identical
speed pass (fixed-depth scores/nodes/root moves verified identical,
perft re-passed). Also below v2.

## v5 — final HEAD
v2's measured-best configuration restored (budget 48000, no bishop
pair; the identical-behaviour speed pass is kept). v2/v3/v4 means
(1276/1219/1220) are within noise of each other; v2 leads on both mean
and points, so it stands as the final entry.
