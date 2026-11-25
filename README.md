# Coverline

**Fully collateralized options for tokenized stocks on Robinhood Chain.** Holders earn a premium by writing covered
calls and cash-secured puts; buyers get defined-risk exposure. Every action shows what you receive or pay, when, and
the worst case, before you sign.

> **Status: testnet only.** Coverline runs on the Robinhood Chain testnet (chain 46630) with test assets that have no
> value. There is no mainnet deployment. This repository is a showcase: it contains no source code. The source
> repositories are private; reviewers can get read access on request.

![Pro trading screen](assets/pro-trade.png)

| Simple view: "Earn on my tokens" | The same ticket on a phone |
| --- | --- |
| ![Simple view](assets/simple-earn.png) | <img src="assets/phone-ticket.png" width="300" alt="Order ticket on a phone"> |

*Screenshots are from the automated browser tests on a local test chain with sample data.*

## What it does
- **Covered calls and cash-secured puts, 100% escrowed.** No margin, no liquidations. Lots, partial fills, bids and
  sell-to-bid, cancel and buy-back-to-close at any time.
- **Oracle settlement with a grace window.** If no price report is close enough to expiry, settlement is deferred
  instead of using a stale price.
- **Standard expiries follow the US trading calendar.** Fridays at the market close, moved for holidays and early
  closes, from the exchange's published table.
- **Plain previews.** The SDK simulates every order first and states: you pay, you receive, when, worst case.
- **Budgeted agents (phase 2a).** An owner can let an agent write or buy within permissions, budgets and an expiry.
  Funds never go to the agent.
- **Honest about issuer controls.** Stock Tokens are tokenized debt securities whose issuer can pause, block or burn
  tokens. If escrowed tokens are burned, payouts are pro rata; the app says so wherever it shows a worst case.

## Architecture
![Architecture](assets/architecture.svg)

| Part | What it is |
| --- | --- |
| Contracts (Solidity, Foundry) | `OptionsMarketV2` (escrow, order book, settlement), `AgentModule` (agent permissions), oracle adapter, fee splitter, read lens |
| SDK (TypeScript) | Typed reads and writes, simulate-before-sign previews, trading calendar, indexer client |
| Indexer and keeper | Reorg-safe indexer with a read-only HTTP API; a keeper that settles and pushes payouts |
| App | Pro and Simple views, Portfolio, Activity, Agents, Settings; desktop and phone |
| UI package | Accessible components (WCAG 2.2 AA checks in CI) |
| MCP server | Lets AI clients read the market and prepare **unsigned** transactions with the full preview. It holds no keys and never sends |

## Testnet deployment (Robinhood Chain testnet, chain 46630)
Phase 2a set with the `AgentModule`, deployed on 2026-11-25. The market was deployed at block 157695146. Test assets
only.

| Contract | Address |
| --- | --- |
| OptionsMarketV2 | `0xE11a90aB3Eba29a99bB89A95C683DA06eccD9CF6` |
| AgentModule | `0x5644449AB177b1Ec67b28Ebf8FF97649F6aAc0D2` |
| ChainlinkOracleAdapter | `0x6F557d7635aFC86c3F9c83cBAaFAc581E6b513d5` |
| FeeSplitter | `0x56F6c23BF2C2Fcb53A082Ff133c2D6D55E460971` |
| MarketLens | `0x907AE1446cE1e29dA308A8358218784551F137F6` |

The quote token and the price feed on the testnet are mocks, used on the testnet only.

**Public testnet demo:**
- Site: https://coverlinefi.xyz
- App: https://testnet.coverlinefi.xyz
- Indexer API: https://testnet-api.coverlinefi.xyz
- MCP endpoint: https://testnet-mcp.coverlinefi.xyz/mcp

The names without `testnet` are reserved for mainnet.

## Engineering (2026-11-16)
| Area | State |
| --- | --- |
| Contracts | 164 tests: unit, fuzz and 12 invariants (solvency, agent budgets); static analysis triaged with no high-severity finding; tests on a fork against real Robinhood Chain assets |
| SDK | 674 tests; a weekly differential run compares the SDK's maths with the contracts: 0 differences |
| Indexer and keeper | Weekly soak with chain reorganisations, deferred settlements and a simulated issuer burn; the end state equals fresh chain reads |
| App | Browser end-to-end tests on recorded data and, weekly, the full demo flow on a fork of the testnet; accessibility checks on every page |
| Process | Small reviewed pull requests, CI on every repository, written product and design decisions |

**Not done yet:** external audit, legal review, mainnet. No users and no TVL: testnet only.

## Availability
Coverline is not available in the US or to US persons, Canada, the United Kingdom or Switzerland, and not to
investors in Cuba, Belarus, Iran, North Korea, Russia, Syria, Ukraine, South Sudan, Sudan, Myanmar or Venezuela.
Further restrictions may apply. Nothing here is an offer or advice.

## Read more
- The article introducing Coverline: https://x.com/0xMelkoreth/status/2120138851842658656

## Access and contact
The source repositories are private. For read access as a reviewer, open an issue in this repository.
