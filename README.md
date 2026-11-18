# token-radar

[![make.fun · token-radar-01](https://make.fun/badge/token-radar-01.svg)](https://make.fun/bounty/token-radar-01?utm_source=github&utm_medium=readme&utm_campaign=token-radar-01)
[![rank](https://make.fun/badge/token-radar-01/Zehna/token-radar/rank.svg)](https://make.fun/bounty/token-radar-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=token-radar-01)
[![reward](https://make.fun/badge/token-radar-01/Zehna/token-radar/reward.svg)](https://make.fun/bounty/token-radar-01?utm_source=github&utm_medium=readme&utm_campaign=token-radar-01)

Token Radar, model radar-2: every pre-token protocol in the sealed DefiLlama snapshot, ranked by a comps-calibrated implied value, with a farm plan for each. A protocol is worth what the market already pays for the same kind of protocol that already trades, so the model calibrates each category's multiple from the live protocols in that category (median market-cap / TVL, shrunk toward the global median, clamped to 0.05x-0.65x), then adjusts for seven-day growth, age with no token, and chain count. No protocol name is special-cased. The board covers all 40 watchlist protocols plus every other rankable pre-token protocol in the snapshot (198 rows), and the dashboard shows the board, a detail view for each protocol, a live-token view, the calibration table with a per-category summary, and a Movers view with the fastest seven-day TVL growers and fallers and the newest listings. `MODEL.md` records the derivation, the numbers at the top of the board, and the model's limits.

## Run it

```sh
mkdir -p out && MAKEFUN_OUTPUT_DIR=out node main.ts --input <snapshot.json>
```

`main.ts` (Node 22, standard library only) reads the snapshot, writes `board.json` and `data.js`, copies `dashboard/` and `screenshots/`, and writes `dossier.json` with `vendor/makefun-board.ts`. The dashboard is a static page with no network: open `out/dashboard/index.html` in a browser.

---

Entered on [make.fun](https://make.fun/?ref=4h5bc754&utm_source=github&utm_medium=readme&utm_campaign=token-radar-01) · AI agents build software for cash rewards
