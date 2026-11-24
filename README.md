# Hi, I'm Spike

I'm a backend engineer, mostly in Go. I build systems that have to keep their state correct when things fail: zk-rollup sequencers, Solana trading backends, Bitcoin indexers, and now agent runtimes.

I have 5 years of production experience, 4+ of them in Web3 infrastructure.

[X / Twitter](https://x.com/0xfakeSpike) · [Email](mailto:isyiming.zhang@icloud.com)

## What I'm building now

[**Ollupi**](https://github.com/0xfakeSpike/ollupi) is a Go framework for **agent cloud services** — running many users' personal agents on shared infrastructure. Instead of a VM or container per agent, it pools the runtime and persists each agent's state: every step is written to the database before it runs, and no scheduling state lives in memory. What that buys:

- **Inactive sessions cost nothing.** A session waiting on a user, an approval or a subagent leaves the execution pool and remains only as database rows.
- **Recoverable at any moment.** After a restart, a release or a crash, sessions continue where they stopped. Tool calls distinguish `pending` from `inProgress`, so a payment or a message is never executed twice.
- **Controlled concurrency.** Slots in one pool, model calls limited per connection to match the provider's account quota; bursts queue instead of failing.
- **Multi-user and sharded.** One database is one shard; a lease keeps a single writer, a standby takes over when it exits, and each user gets their own slots, quota and sandbox. Commands, files and MCP servers go through `sandbox.Box` — seatbelt locally, and a cloud sandbox only takes implementing one interface.
- **Measurable, not just benchmarked.** Besides the scheduling and persistence benchmarks below, the repo runs Ollupi on Terminal-Bench 2.0 tasks through Harbor, to compare each version of the runtime with the last.

Measured on Ollupi v0.18.0 (2026-11-19), on an 8 GB M1 with one SQLite database and simulated models and tools, one run each:

| Test | Result |
| --- | --- |
| 50,000 parked sessions | 34 MB of resident memory and about 7 KB of disk each, against 17.7 MB per session for one process per agent |
| 1,536 active sessions | Median turn 5% slower than running alone. Past that, the store's single SQLite connection is the bottleneck, and shards split it: 4,096 sessions on 1, 2 and 4 shards run 84.9, 116.3 and 144.4 turns a second |
| 3,000 turns submitted at once, quota 256 | All complete with no 429s, in 1.04× the time the quota allows; pi, pi-durable and DeepSeek Harness complete 483–1,764 |
| One user filling every slot | With a per-user slot limit, other users wait 8 ms instead of 206 s |
| 30 random `kill -9`s | All 600 turns completed, none of the 110 interrupted tool calls ran again, and a standby takes over a shard in 1.3 s |

The [method and raw results](https://github.com/0xfakeSpike/ollupi/tree/main/bench) are in the repo, along with a [comparison against pi and DeepSeek Harness](https://github.com/0xfakeSpike/ollupi/tree/main/bench/compare); the [docs site](https://0xfakespike.github.io/ollupi-docs/) covers the protocol and client development in Chinese and English.

Two related projects:

- [**Ollupi Desktop**](https://github.com/0xfakeSpike/ollupi-desktop) is the macOS app built on Ollupi: approvals and permission policies, scheduled tasks, memory, a browser the agent and the user share, file / git / terminal panels, a context-usage panel, and a Feishu bot for working from your phone.
- [**coding-standards**](https://github.com/0xfakeSpike/coding-standards) is a Claude Code skill that holds agents to an opinionated set of engineering rules — 1,676 of them, in Chinese and English. *Now it's the AI's turn to suffer.*

## Writing

New long-form pieces go on [X](https://x.com/0xfakeSpike), in Chinese:

| Date | Piece | Links |
| --- | --- | --- |
| 2026-11-08 | 水在青天云在瓶：agent 的身首错位现象 | [X](https://x.com/0xfakeSpike/status/2119341398269083908) · [中文](https://0xfakespike.github.io/ollupi-docs/zh/articles/head-in-the-cloud) · [English](https://0xfakespike.github.io/ollupi-docs/articles/head-in-the-cloud) |
| 2026-11-04 | Agent 的 C10K 时刻 | [X Article](https://x.com/0xfakeSpike/status/2117838116614725755) |

Older posts on my archived blog, `0xfakespike.github.io`:

- [AMM math, from CPMM to DLMM](https://0xfakespike.github.io/#/ammcpmmdlmm)
- [Iceberg: a privacy swap built on ZK mixing](https://0xfakespike.github.io/#/iceberg-zkswap)
- [Parsing of the AVM](https://0xfakespike.github.io/#/parsing-of-the-avm)

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

## Toolbox

- **Backend:** Go, PostgreSQL, SQLite, Redis, Kafka, gRPC, WebSockets
- **Web3:** Solana, Bitcoin, EVM, zk-rollups, AMMs, wallet infrastructure
- **Agents:** MCP, durable execution, sandboxing, context management, human approval flows

## Open to work

I'm interested in backend roles in:

- AI agent infrastructure and developer tooling
- Web3 infrastructure, wallets, indexers, and transaction systems
- Privacy-preserving trading and execution infrastructure
