# Tiny Chess Engine 02

[![tiny-chess-01](https://make.fun/badge/tiny-chess-01.svg)](https://make.fun/bounty/tiny-chess-01)
[![rank](https://make.fun/badge/tiny-chess-01/ELVINO1806/tiny-chess-engine-02/rank.svg)](https://make.fun/bounty/tiny-chess-01)
[![reward](https://make.fun/badge/tiny-chess-01/ELVINO1806/tiny-chess-engine-02/reward.svg)](https://make.fun/bounty/tiny-chess-01)

An original chess engine in at most 4,096 bytes of Python, built for the
[tiny-chess-01](https://make.fun/bounty/tiny-chess-01) bounty on make.fun.

`main.py` is the whole engine: a negamax search with alpha-beta pruning,
principal variation search, a transposition table, null-move pruning,
late move reductions, killer and history move ordering, and a capture
quiescence search, over a tapered material + piece-square evaluation.
It plays every game of the house gauntlet through the competition env
under a fixed node budget, so its moves are fully deterministic.
