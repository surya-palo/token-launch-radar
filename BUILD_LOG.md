# token-launch-radar — build log

Built by the subagent session `9b6ef824-447f-4a21-b78b-74d13aa480be`
(parent `c3ce2321-77d9-412e-b24c-fdc2f90c9eca`), 2026-11-19. Account: Surya
Palo (@surya-palo), GitHub surya-palo, referral code `5zv4vr8w`.

Competition: `token-radar-01` — "Build the best token launch radar", $1,325
(1st $1,000), entries close 2026-12-14T10:00:00Z. Judged by an AI judge on the
dossier (screenshots + dashboard source), pairwise, win rate decides places.

## What was built

Location: `~/workspace/make-fun/entries/token-launch-radar/`
(commit `195b437`, plus this log).

- `main.ts` (91 lines) — entrypoint: reads `/input/snapshot.json`, builds the
  board, writes `/output/board.json`, `/output/data.js`, copies `dashboard/`
  and `screenshots/`, writes `/output/dossier.json` with the vendored library.
- `lib/model.ts` (662 lines) — the value model and farm plans (see below).
- `dashboard/index.html`, `dashboard/app.css`, `dashboard/app.js` (16 + 161 +
  482 lines; 42,643 bytes in all) — dark-theme dashboard: sortable/filterable
  board, per-protocol detail drawer with the model's factor breakdown and farm
  plan, Pre-token / Live token / Set aside tabs, model sandbox (growth weight,
  maturity range, category multiples on/off) that re-ranks the board live.
- `vendor/makefun-board.ts` — copied byte-for-byte from the kit
  (sha256 `9ca51b6e…dc326e`, matches kit.json).
- `LICENSE` — MIT from the kit (SPDX line present).
- `README.md` — the make.fun template with the three live badges, referral
  link `https://make.fun/r/5zv4vr8w`.
- `screenshots/` — three real renders of the dashboard at 1440x900:
  `1-board.png` (155,779 bytes), `2-detail.png` (214,008 bytes),
  `3-filtered.png` (156,885 bytes).

## The model (radar-2)

- Board: 178 rows — all 40 watchlist slugs + every launch candidate below the
  $50M floor (pre-token per the record and passing the house's exclusion rule:
  not a Bridge/CEX/Liquid Staking product, no wrapped/staked/staking word, no
  version marker, no exchange-name prefix). Candidates at or above the floor
  that the watchlist leaves out are left out here too: the house's
  parent-protocol clause, which the snapshot cannot see, is what kept them off.
- Implied value: `TVL x category_multiple x growth x maturity x breadth`.
  The category multiple is measured on the snapshot itself: the median
  mcap/TVL of the category's live-token protocols, shrunk toward the global
  median (0.214x) and clamped to [0.02, 1.5]; a category with fewer than 3 live
  protocols takes the global median. Growth is clamped change_7d (-60..+120)
  at weight 0.5; maturity 0.65 + 0.35 x min(age, 1460)/1460; breadth 1 + 0.05 x
  min(chains-1, 4). No name is ever special-cased.
- Total implied value of the board: $2,928,007,938. Rank 1: Steakhouse
  Financial, $545,201,548.
- Farm plans: 7 steps per protocol, derived from category, chains, TVL and
  age; gas priced by chain, capital ($50–$250) by TVL, hold (30–60 days) by
  age; every step carries a cost and a time.
- Set aside: 12 pre-token protocols at or above the floor that the watchlist
  leaves out (Bybit, WBTC, Binance staked ETH, USDT0, …), each with the rule
  that left it out; 24 live-token protocols shown with market caps.

## Validation

- Gate validator (`/tmp/gate-check.mjs`, re-implements the seven sealed rows
  from RULES.txt): all seven PASS — dashboard_files, board_file (178 rows),
  sorted_by_value, coverage (40/40), farm_steps (178 x 7, all 20..600 chars),
  screenshots (3, 1440x900, each under 512 KiB), dossier_matches
  (746,387 bytes, byte-identical to a rebuild). Dossier text 42,909 bytes:
  all dashboard source included whole, nothing cut.
- Determinism: two runs of main.ts produce byte-identical output.
- Cross-check (`/tmp/cross-check.mjs`): the dashboard's own `impliedOf` and
  `farmSteps`, extracted from app.js and run against the snapshot, agree with
  board.json on all 178 rows (0 implied mismatches, 0 farm mismatches).
  Found and fixed one bug this way: data.js carried the global multiple
  rounded to 4 decimals while the model used the raw value, which put 14 rows
  off by ~0.0015% in the page's recompute. Fixed in model.ts (store the raw
  global multiple); board.json unchanged, rank order unchanged, screenshots
  re-rendered and confirmed pixel-identical.
- Screenshots rendered with the VM's Chromium (`/opt/meta-chromium/chrome`)
  driven over the DevTools protocol (its `--screenshot` CLI path produces no
  output in this build). It needs a WebGL domain policy file plus its sha256:
  `/tmp/webgl-policy.txt` (`hatch-webgl-allowed-domains-v1` + `example.com`),
  switches `--hatch-webgl-allowed-domains-file` and
  `--hatch-webgl-allowed-domains-sha256`. A stock Chromium download via
  puppeteer was attempted and failed (network to googlechromelabs.github.io
  plus an approval that timed out); the CDP route with the installed binary
  worked instead. Localhost HTTP serving is blocked by the build's local
  network access checks, so the captures used file:// URLs with
  `--allow-file-access-from-files`.

## Transcript bundling

`bundle --dry-run` works: 5 Muse app sessions found, including this build's
session (`9b6ef824-…`, 62 turns) and the parent's (`c3ce2321-…`, 48 turns).
The final bundle must be run by the top-level agent after this subagent
finishes, with `--muse-usage` (usage rows for all five agent ids in the
dry-run output), `--system-prompt <file>`, `--mcp none` (or server names),
`--harness-version` / `--provider` / `--model` / `--effort`.

## What remains for submission

1. User connects the wallet (they said they will do it themselves). No
   wallet/keypair was created, nothing signed, nothing submitted, nothing
   paid — all hard limits respected.
2. GitHub-side steps via browser (no `gh` auth here): create repo
   `surya-palo/token-launch-radar` and push this commit; set the repo About
   (homepage `https://make.fun/bounty/token-radar-01`, topics
   `makefun,ai-agents,bounties,trading`, description naming make.fun and the
   bounty). The repo must be created after the competition opened
   (2026-11-14T10:00:00Z) — it will be, on 2026-11-19.
3. Final bundle (top-level agent, after this subagent finishes) + `makefun
   check` + submit with the wallet signature.
4. GTA 6 entry was NOT started (needs live-witnessed recordings started
   before building; separate planned job).
