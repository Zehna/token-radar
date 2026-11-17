# Token Radar, model radar-2 — how the ranking is built

This note records the model behind the board in `board.json`, so a reader
can check every number without running anything. All figures come from the
sealed snapshot (`input/snapshot.json` in the bounty kit): 758 protocols
from DefiLlama's protocol index, fetched 2026-11-14, with a watchlist of
40. Nothing is fetched live, and no protocol name is special-cased.

## What is ranked

A protocol is rankable when it has no token on record, is not an exchange,
is not a versioned pool, and is not a wrapped, staked or bridged product of
a token that already trades. That last exclusion is the same rule the
watchlist uses: WBTC is Bitcoin on Ethereum, Binance staked ETH is ETH in a
staking product, and a bridge token is a token that already trades in
another form, so none of them is a launch candidate. 198 protocols pass:
the 40 on the watchlist plus 158 below the $50M TVL floor. 24 live-token
protocols are set aside; they are not ranked, they calibrate the model.

## The implied value

For a protocol with TVL `T`, category `C`, seven-day change `g`,
age in days `a`, and `n` chains:

```
implied = round( T x comp(C) x growth(g) x maturity(a) x chains(n) )
```

- `comp(C)` is the category's multiple: the median market-cap / TVL of
  the live-token protocols in that category, shrunk toward the global
  median (0.2138x) and clamped to 0.05x-0.65x. A category with fewer than
  three live protocols uses the global median outright, because one
  outlier must not price a whole category: a single prediction market
  whose market cap dwarfs its TVL, or a single risk curator, would
  otherwise set the price for every pre-token protocol in its category.
- `growth(g) = 1 + clamp(g, -60, 120) / 100 x 0.6`. A protocol growing
  fast is worth more than a shrinking one at the same TVL, but the
  multiplier is capped so a 400% week from a tiny base does not dominate
  the board.
- `maturity(a) = 0.6 + min(a, 1095) / 1095 x 0.4`. Age is the time since
  the protocol was first listed, the best record of how long it has run
  with no token. A protocol listed three years ago has earned the full
  maturity factor; a protocol listed a month ago has not, and an
  unlisted age is treated as 30 days.
- `chains(n) = 1 + min(n - 1, 4) x 0.03`. A protocol live on more chains
  reaches more users, but the bonus is small and capped at four extra
  chains, so a chain count cannot rescue a weak TVL.

## What the top of the board looks like

The highest implied values belong to the vault curators and capital
allocators with the largest TVL — Steakhouse Financial at $519.6M, then
Sentora Curator, Gauntlet and Veda — because their TVL is in the billions
and their comp is the global median, not a category outlier. The board's
total implied value is $3.82B across 198 rows. The Movers view shows the
other end of the story: the fastest seven-day TVL moves, where a small
protocol can grow 400% in a week and still sit far down the ranking.

## The farm plan

Each row carries six steps, derived from the same four inputs as the
implied value, never from the protocol's name:

1. A fresh wallet, its seed phrase offline, and a warning never to
   connect from a link in a message.
2. The category-specific action (supply, provide liquidity, bridge,
   stake, deposit, mint, buy cover, or place a market position) with a
   capital tier — $250 at $5B TVL or more, $150 at $1B, $100 at $100M,
   otherwise $50 — and gas of about $12 on Ethereum, $4 elsewhere.
3. One qualifying action a week, so the wallet stays in the protocol's
   activity records.
4. A hold of 30 to 60 days, longer for younger protocols.
5. A check of the protocol's own points or rewards page before
   withdrawing, and never paying to claim.
6. A farm log, and an exit in tranches.

## Limits, stated plainly

- Implied value is a model, not a price. It says nothing about whether
  or when a token is issued, and nothing about the token's launch price.
- The snapshot is one day, 2026-11-14. A protocol that launched its
  token after that date, or whose TVL moved sharply since, is scored on
  stale figures.
- TVL is taken as reported by DefiLlama. A curator's TVL is the deposits
  in the vaults it curates, which is not the same kind of TVL as a
  lending market's, and the model does not adjust for that.
- The comp multiple is a median, not a guarantee: two protocols in the
  same category can trade at very different multiples, and the board
  cannot see why.
- The farm plan is a generic interaction pattern, not investment advice,
  and it assumes the protocol's own site is the authority on what counts.
