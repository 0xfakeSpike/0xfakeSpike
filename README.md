# Hi, I'm Spike

I'm a backend engineer, mostly in Go. I build systems that have to keep their state correct when things fail: zk-rollup sequencers, Solana trading backends, Bitcoin indexers, and now agent runtimes.

I have 5 years of production experience, 4+ of them in Web3 infrastructure.

[Website](https://0xfakespike.github.io) · [X / Twitter](https://x.com/0xfakeSpike) · [Email](mailto:isyiming.zhang@icloud.com)

## What I'm building now

[**Ollupi**](https://github.com/0xfakeSpike/ollupi) is a Go agent framework for running many personal agents on shared infrastructure. Instead of giving each agent its own VM or container, it pools the runtime and writes every step to the database before executing it. A session waiting on a user costs only a database row, and a restart continues from where it stopped.

Results from an 8 GB M1 with simulated models and tools, measured before the multi-user and sharding work:

| Test | Result |
| --- | --- |
| 50,000 parked sessions | About 14 MB of extra memory and 2.3 KB of disk per session |
| 2,048 active sessions | Median turn 2% slower than running alone, using 407 MB of memory |
| 3,000 turns submitted at once | No failures or 429s, finished within 1.03× of the theoretical minimum |
| 90 random `kill -9`s | All 960 turns completed, and no tool call ran twice |

The [method and raw results](https://github.com/0xfakeSpike/ollupi/tree/main/bench) are in the repo, along with a [comparison against pi and DeepSeek Harness](https://github.com/0xfakeSpike/ollupi/tree/main/bench/compare).

Two related projects:

- [**Ollupi Desktop**](https://github.com/0xfakeSpike/ollupi-desktop) is the macOS app built on Ollupi, with approvals, scheduled tasks, memory, a shared browser, and a Feishu bot for working from your phone.
- [**coding-standards**](https://github.com/0xfakeSpike/coding-standards) is a Claude Code skill that holds agents to an opinionated set of engineering rules, in Chinese and English. *Now it's the AI's turn to suffer.*

## Production experience

- **zk-rollup:** Maintained the backend of [zkBNB](https://github.com/bnb-chain/zkbnb), including sequencing, deterministic state execution, proof orchestration, and BNB Chain settlement. I'm the project's second-largest contributor by commits.
- **Solana trading:** Owned the backend architecture, implementation, APIs, and initial production operation of a Solana trading product at a centralized exchange.
- **Wallets and funds:** Worked on multi-chain wallet execution, key-custody migration, and cross-chain fund workflows.

## Open source

| Project | What it is |
| --- | --- |
| [**atomicals-go**](https://github.com/atomicals-community/atomicals-go) | Crash-safe, fork-aware Atomicals indexer for Bitcoin that runs on a pruned node with PostgreSQL. Includes an FT-splitting proposal adopted into the protocol's official implementation. 88 stars. |
| [**SolRoute**](https://github.com/Solana-ZH/solroute) | Go SDK for working with Solana AMMs directly: pool discovery, quoting, routing, and instruction building across Raydium, Meteora, and PumpSwap. 50+ stars and a 2026 Solana Foundation ecosystem grant. |
| [**polymarket-go**](https://github.com/0xfakeSpike/polymarket-go) | Go SDK for the Polymarket CLOB, with the `pmctl` CLI and an MCP server. Installs through Homebrew. |
| [**Global Liquidity Monitor**](https://0xfakespike.github.io/global_liquidity/) | Live dashboard of USD and JPY liquidity, Treasury markets, and cost of capital, built from public Fed, BOJ, and Treasury data. |

**Hackathons:** [Overheat](https://github.com/OverheatSG) is an on-chain AI oracle for Solana and EVM that won 2nd place at a Solana hackathon with a four-person team. [Iceberg](https://github.com/Iceberg-trade/iceberg-core) is a privacy-swap prototype on Arbitrum One using Groth16 proofs and 1inch execution.

## Writing

Mostly in Chinese, on [my website](https://0xfakespike.github.io):

- [AMM math, from CPMM to DLMM](https://0xfakespike.github.io/#/ammcpmmdlmm)
- [Reading the Raydium CLMM and Meteora DLMM code](https://0xfakespike.github.io/#/raydiumclmmmeteoradlmm)
- [Iceberg: a privacy swap built on ZK mixing](https://0xfakespike.github.io/#/iceberg-zkswap)

## Toolbox

- **Backend:** Go, PostgreSQL, SQLite, Redis, Kafka, gRPC, WebSockets
- **Web3:** Solana, Bitcoin, EVM, zk-rollups, AMMs, wallet infrastructure
- **Agents:** MCP, tool use, durable execution, context management, human approval flows

## Open to work

I'm interested in backend roles in:

- AI agent infrastructure and developer tooling
- Web3 infrastructure, wallets, indexers, and transaction systems
- Privacy-preserving trading and execution infrastructure
