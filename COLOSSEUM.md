# SuperView (multi-desk)

SuperView is a social investing product: write a belief, let an agent build a **benchmarked basket** of tokenized stocks (and a separate **memes** desk), publish as a view, and track performance vs the S&P 500.

Stock routing spans **Solana**, **Robinhood Chain**, **Base**, and **Monad**.

## What to try

1. Local setup (`README.md`).
2. **Stocks**: compose a view; research screens catalogs on multiple desks and picks one listing per company.
3. **Memes**: switch world to memes; Solana launchpad catalog and risk gates apply.
4. **Sky lens**: optional astrology chart path for thematic views.
5. **Masumi agent** (`apps/masumi-agent`): hire the research agent via Cardano Preprod (optional; separate from in-app paper/live).

## Architecture highlights

- Committee-style research pipeline (interpret → screen → diligence → construct → critic)
- Venue router scores liquidity, halt status, and wallet desk affinity
- Paper USD ledger per pocket; live execution gated by feature flags and Privy or passkey signers
- Live quote stream and vs-SPY marks on published views

## Walkthrough checks

- End-to-end **paper** loop: view → basket → invest → portfolio mark
- Feed / trending shows live % moves
- Multi-chain chips on holdings where listings exist
- Local setup from README with `.env.example` only (no secrets in git)
