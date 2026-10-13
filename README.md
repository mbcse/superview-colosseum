# SuperView

**Say what you believe about the world. Watch it play out.**

SuperView closes the gap between having a market opinion and having a **testable book**. Users write a short view in plain language. An agent interprets the thesis, screens tokenized stock catalogs on multiple chains, sizes a basket, and publishes it as a social object with live marks versus the S&P 500. Followers can comment, copy holdings, and invest in **paper** (default) or **live** when flags and wallets are enabled.

Multi-desk routing, memes world, and the optional Cardano agent are documented in [COLOSSEUM.md](./COLOSSEUM.md).

---

## Product surfaces

| Surface | What happens there |
| --- | --- |
| Home / trending | Published views ranked by live performance, not follower count |
| New view | Free-text belief triggers async research |
| View page | Thesis, holdings, vs-SPY chart, comments, copy, invest |
| Portfolio | Pocket balances, fills, per-name marks, rebalance proposals |
| Wallet | Privy email/social or passkey (Mera on supported desks); per-chain balances when live |
| Memes world | Solana launchpad catalog, separate risk gates from stocks |

---

## Multi-desk stocks

The agent picks **companies**. Code picks **one listing per company** across active stock desks:

- **Solana** (xStocks and related listings)
- **Robinhood Chain** stock tokens
- **Base**
- **Monad** (chain 143, xStocks, AUSD)

Venue routing scores liquidity, halt status, and which desk the user’s wallet is on. Research logs show which catalog rows survived screening.

---

## Research pipeline

1. **Interpret** the belief into mechanism, horizon, assumptions, and investable angles.
2. **Screen** each desk’s active catalog; map web-discovered names back to tradable symbols.
3. **Diligence** each candidate (notes, directness, purity, risk).
4. **Construct** a 5-12 name basket with role and concentration limits.
5. **Critic** a separate model can reject and force another construction pass.
6. **Publish** the view; quotes and day change stream to the UI.
7. **Monitor** (worker) can propose trims, adds, or rebalances when the thesis drifts.

Default models are documented in `.env.example` (`gpt-4o` research, `gpt-4o-mini` fast paths, Haiku critic).

---

## Execution modes

| Mode | Behavior |
| --- | --- |
| Paper | USD ledger per pocket; fills priced from live oracle / issuer marks |
| Live | Gated by `APP_MODE`, `live_trading`, and wallet type; unsigned txs prepared server-side, signed client-side (Privy or Mera passkey) |

Memes desk uses its own catalog sync and paper/live rules on Solana.

---

## Masumi agent (optional)

`apps/masumi-agent` exposes a hireable research agent on **Cardano Preprod** via Sokosumi job starts. It reuses the same research stack but is separate from the in-app paper button path.

```bash
pnpm dev:masumi-agent
```

See `apps/masumi-agent/README.md` for job input shape.

---

## Monorepo map

```
apps/web           Next.js UI
apps/api           Fastify API + SSE quote stream
apps/worker        Catalog sync, quotes, research jobs
apps/masumi-agent  Cardano agent entrypoint (optional)
packages/ai        Committee + monitor agents
packages/core      Baskets, ledger, access control, vs-SPY
packages/markets   xStocks, Robinhood, Kuru, Aurora adapters
packages/db        Prisma + seed
packages/shared    Worlds, chain config, catalog search
packages/config    Env validation
```

---

## Run locally

```bash
pnpm install
cp .env.example .env
docker compose up -d
pnpm db:generate && pnpm db:migrate && pnpm db:seed
pnpm dev
```

| Service | URL |
| --- | --- |
| Web | http://localhost:3000 |
| API | http://localhost:4000 |

`OPENAI_API_KEY` is required for research. Privy, Anthropic, 0x, and desk-specific keys are optional until you test login or live trading.

```bash
pnpm test
```

---

## Walkthrough

1. Seed + `pnpm dev`. Confirm API health.
2. Create a stocks view with a clear sector thesis (energy, AI infra, etc.).
3. Wait for research to finish; open holdings and note **multi-chain** chips where listings exist.
4. Paper invest; open **Portfolio** and confirm marks move with the quote stream.
5. Open **Trending** and confirm % day change is non-static on published views.
6. Optional: switch to **Memes** world and run a second, smaller view.
7. Optional: start `masumi-agent` and fire a Sokosumi job with a 14-26 hex purchaser id.
