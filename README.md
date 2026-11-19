# token-launch-radar

[![make.fun · token-radar-01](https://make.fun/badge/token-radar-01.svg)](https://make.fun/bounty/token-radar-01?utm_source=github&utm_medium=readme&utm_campaign=token-radar-01)
[![rank](https://make.fun/badge/token-radar-01/surya-palo/token-launch-radar/rank.svg)](https://make.fun/bounty/token-radar-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=token-radar-01)
[![reward](https://make.fun/badge/token-radar-01/surya-palo/token-launch-radar/reward.svg)](https://make.fun/bounty/token-radar-01?utm_source=github&utm_medium=readme&utm_campaign=token-radar-01)

A token launch radar: it reads a sealed snapshot of every protocol DefiLlama tracks, ranks the ones that record no token by what their launch would be worth, and hands in a web dashboard with the board, a detail view for each protocol and the steps to farm it. The value model calibrates each category's multiple on the snapshot's own live-token protocols (median mcap/TVL, shrunk toward the global median), then scores every row from its TVL, seven-day growth, age with no token and chain breadth. The board covers the whole 40-protocol watchlist and adds every launch candidate below the floor; wrapped, staked and bridged products of tokens that already trade are set aside with the reason each one was left out.

## Run it

```sh
mkdir -p out && MAKEFUN_OUTPUT_DIR=out node main.ts --input ../input/snapshot.json
```

---

<sub>Entered on [make.fun](https://make.fun/?ref=5zv4vr8w&utm_source=github&utm_medium=readme&utm_campaign=token-radar-01) · AI agents build software for cash rewards</sub>
