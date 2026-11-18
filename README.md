# Tiny Chess — a 4 KB chess engine

An original chess engine that fits in a single `main.py` of **3,330 bytes**,
built for the [make.fun](https://make.fun/bounty/tiny-chess-01) `tiny-chess-01`
bounty. The whole repository snapshot stays well under the 8,192-byte cap.

[![make.fun bounty](https://img.shields.io/badge/make.fun-tiny--chess--01-blue)](https://make.fun/bounty/tiny-chess-01)
[![enter with my referral](https://img.shields.io/badge/referral-qtgxrqfw-green)](https://make.fun/r/qtgxrqfw)
[![license: MIT](https://img.shields.io/badge/license-MIT-yellow)](LICENSE)

## How it plays

The engine stores every position from the side-to-move's point of view on a
120-square board; making a move rotates the board 180 degrees and swaps case,
so move generation, castling, pawn direction and piece-square tables are all
written once, for one colour. Positions are immutable and the evaluation is
maintained incrementally through the rotation.

Search is a negamax alpha-beta with principal-variation search, a transposition
table, null-move pruning, late-move reductions and a quiescence search over
captures and promotions (with full evasion search when in check). The evaluation
is a tapered midgame/endgame blend of material and piece-square tables. A fixed
budget of 65,000 nodes per move keeps play fully deterministic: no wall clock,
no randomness anywhere in move choice.

Correctness was verified before packing: legal move sets match python-chess
exactly over 13,721 plies of random games plus tactical test positions
(en-passant pins, castling through check, underpromotions), perft counts match
the standard reference values on five positions, and the incremental score
never drifts from a from-scratch evaluation.

## Entry format

`main.py` is the whole engine. Its bytes are an lzma-compressed, base85-encoded
payload behind a two-line bootstrap — the compression the bounty rules
explicitly allow — and it talks to the trial environment over
`$MAKEFUN_ENV_URL` using only the Python standard library.

## License

MIT — see [LICENSE](LICENSE).
