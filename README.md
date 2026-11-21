# Token Radar

A token launch radar for the
[make.fun token-radar-01 bounty](https://make.fun/c/token-radar-01): every
pre-token protocol in the sealed DefiLlama snapshot (2026-11-14), ranked by an
implied value modelled from its own figures, with a concrete farm plan for
each one.

[![make.fun · token-radar-01](https://make.fun/badge/token-radar-01.svg)](https://make.fun/bounty/token-radar-01?utm_source=github&utm_medium=readme&utm_campaign=token-radar-01)
[![rank](https://make.fun/badge/token-radar-01/FiluFafayo/token-radar/rank.svg)](https://make.fun/bounty/token-radar-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=token-radar-01)
[![reward](https://make.fun/badge/token-radar-01/FiluFafayo/token-radar/reward.svg)](https://make.fun/bounty/token-radar-01?utm_source=github&utm_medium=readme&utm_campaign=token-radar-01)

## The board

198 rows: all 40 protocols on the house watchlist, plus 158 more pre-token
protocols below the $50M floor that pass the same strict filter — no token on
record, and not an exchange, a bridge, a staking wrapper, a versioned pool, or
an exchange's own product. Protocols whose token already trades are set aside
in a Live view with their market caps, never mixed into the ranking.

## The model (radar-2)

```
implied = round( tvl_usd x category_multiple
                 x exp(clamp(change_7d, -80, 200) / 100 x 0.35)
                 x (0.55 + ln(1 + age_days) / ln(1 + 1460) x 0.45)
                 x (1 + 0.05 x min(chains - 1, 4)) )
```

Category multiples: trading venues (Dexs, perpetuals, derivatives, options,
prediction markets) 0.40, credit (lending, CDP, yield, curators, allocators)
0.30, everything else 0.22. Growth compounds instead of adding linearly; age
is the days since DefiLlama first listed the protocol, with a log shape that
saturates after about four years. No protocol name is special-cased, and the
same function scores every row. A protocol "records no token" — the snapshot
is not proof it has none.

## The dashboard

A static page with no network: a sortable, filterable board (search, category,
chain, watchlist-only), a top-five spotlight, a detail view per protocol with
the model's own inputs and the page's recomputation of the row, and a six-step
farm plan per protocol with costs and times. Farm plans are derived from each
protocol's category, chains, TVL and age.

## Run it

```sh
mkdir -p out && MAKEFUN_OUTPUT_DIR=out node main.ts --input <snapshot.json>
```

`main.ts` (Node 22, standard library only) reads the sealed snapshot, writes
`board.json`, `data.js`, `dashboard/` and `screenshots/` to the output
directory, and writes `dossier.json` with the vendored make.fun library. It
runs twice at grading and writes byte-identical output both times.
