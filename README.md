# Token Radar — pre-token protocols ranked by implied value

[![token-radar-01](https://make.fun/badge/token-radar-01.svg)](https://make.fun/bounty/token-radar-01)
[![rank](https://make.fun/badge/token-radar-01/patriarkma/token-radar-01/rank.svg)](https://make.fun/bounty/token-radar-01)
[![reward](https://make.fun/badge/token-radar-01/patriarkma/token-radar-01/reward.svg)](https://make.fun/bounty/token-radar-01)

An entry for [make.fun's token-radar-01 bounty](https://make.fun/bounty/token-radar-01): a radar
that finds protocols whose token does not exist yet, ranks them by an implied value, and shows a
reader how to farm each one.

## The model (radar-2)

Every row is scored by one function of the sealed snapshot's own figures — TVL, seven-day growth,
absolute dollar flow, age with no token, chain spread, and sector — never by a hand-picked name:

```
implied = round( tvl x 0.22 x growth x flow x maturity x chains x sector )
growth   = 1 + clamp(change_7d, -60, 150) / 100 x 0.6
flow     = 1 + 0.15 x clamp(tvl x change_7d / 100 / $1B, -1, 1)
maturity = 0.6 + min(age_days, 1095) / 1095 x 0.4
chains   = 1 + 0.04 x min(chains - 1, 5)
sector   = 1.10 trading and lending infra, 1.05 yield and issuance, 0.90 bridges, 1.00 otherwise
```

The board covers the whole 40-protocol watchlist plus the 260 strongest pre-token protocols below
the $50M floor, ranked by value. Protocols whose token already trades are set aside in a Live
token view with their market caps shown. Each row carries a seven-step farm plan with its cost and
time, derived from the protocol's own category, chains, TVL and age.

## Run it

```sh
mkdir -p out && MAKEFUN_OUTPUT_DIR=out node main.ts --input <snapshot.json>
```

`main.ts` (Node 22, standard library only) reads the snapshot, writes `board.json`, the dashboard's
`data.js`, copies `dashboard/` and `screenshots/` to the output, and writes `dossier.json` with the
vendored make.fun library. The dashboard is a static page with no network: open
`out/dashboard/index.html` in a browser.

## Layout

- `main.ts` — the entrypoint: snapshot in, board and dossier out
- `lib/model.ts` — the radar-2 model, the board builder and the farm plans
- `vendor/makefun-board.ts` — make.fun's dossier library
- `dashboard/` — the offline dashboard (`index.html`, `app.css`, `app.js`)
- `screenshots/` — the board, a protocol's detail view and a filtered view

---

Built for [make.fun](https://make.fun) — join with invite code `9g59dt7w`: https://make.fun/r/9g59dt7w
