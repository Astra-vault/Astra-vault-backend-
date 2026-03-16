# Astra-vault-backend

# AstraVault Backend (Rust)

[![Rust](https://img.shields.io/badge/Rust-1.80+-orange)](https://www.rust-lang.org)
[![Stellar](https://img.shields.io/badge/Stellar-SDK-blue)](https://github.com/stellar/rust-stellar-sdk)
[![Axum](https://img.shields.io/badge/Axum-0.7.x-black)](https://github.com/tokio-rs/axum)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

Rust backend for **AstraVault** — a privacy-first real-world asset (RWA) tokenization platform on Stellar.

This service provides:
- REST/JSON API for frontend communication
- Soroban contract invocation & transaction building
- Horizon & Soroban RPC queries
- Event polling & streaming (contract events)
- Mock oracle endpoints (satellite data, climate verification)
- ZK proof submission & status checking

## Features

- Async-first with Tokio & Axum
- Secure transaction signing (via environment or secure key management)
- Rate limiting, CORS, structured logging
- OpenAPI / Swagger support (via utoipa)
- Testnet / mainnet switch via config
- Graceful shutdown & health checks

## Tech Stack

- **Runtime**: Rust 1.80+
- **Web Framework**: Axum 0.7.x
- **Stellar Integration**: `stellar-sdk` (or `soroban-client` + `stellar-strkey`)
- **Async Runtime**: Tokio
- **Logging**: tracing + tracing-subscriber
- **Config**: figment or envy
- **OpenAPI**: utoipa + utoipa-swagger-ui
- **Testing**: tokio::test + reqwest

## Quick Start

### Prerequisites
- Rust 1.80+ (stable)
- Funded Stellar testnet account (recommended)
- `cargo` and `rustup`

### Setup

```bash
git clone https://github.com/kelechi-frankfurt/astravault-backend-rust.git
cd astravault-backend-rust

# Install dependencies
cargo build

# Copy example config
cp .env.example .env
```

### Configure `.env`

```env
# Network: testnet | futurenet | public
STELLAR_NETWORK=testnet

# RPC & Horizon URLs (auto-selected for testnet/public)
HORIZON_URL=https://horizon-testnet.stellar.org
SOROBAN_RPC_URL=https://soroban-testnet.stellar.org

# Signing key (testnet only – never use mainnet here!)
STELLAR_SECRET_KEY=S...

# Server
PORT=3000
RUST_LOG=info,axum=debug
```

### Run the Server

```bash
# Development (with hot reload via cargo-watch optional)
cargo run

# Or with logging
RUST_LOG=debug cargo run
```

Server available at `http://localhost:3000`

### Example Endpoints

| Method | Path                        | Description                              |
|--------|-----------------------------|------------------------------------------|
| GET    | `/health`                   | Server & Stellar connection status       |
| GET    | `/account/{pubkey}`         | Balances, assets, contract data          |
| POST   | `/invoke`                   | Build, sign & submit Soroban invocation  |
| GET    | `/events`                   | Recent contract events (paginated)       |
| POST   | `/zk/submit-proof`          | Submit ZK proof for verification         |
| GET    | `/oracle/satellite`         | Mock satellite data endpoint             |
| GET    | `/oracle/climate`           | Mock climate impact verification         |

OpenAPI docs: `http://localhost:3000/docs` (if utoipa-swagger-ui enabled)

## Folder Structure

```
├── src/
│   ├── api/                # Axum routes & handlers
│   ├── stellar/            # Stellar SDK wrappers, tx building, signing
│   ├── services/           # Business logic (oracle mocks, event polling)
│   ├── models/             # DTOs, request/response structs
│   ├── config/             # Environment & config loading
│   ├── utils/              # Logging, error handling, tracing
│   └── main.rs             # App entry point
├── tests/                  # Integration & unit tests
├── migrations/             # (future) DB schema if adding Postgres
├── .env.example
├── Cargo.toml
└── Cargo.lock
```

## Development Guidelines

- Use `async/await` everywhere
- Validate inputs with `serde` + custom extractors
- Never hard-code secrets — use `.env` or secure vault
- Conventional commits preferred
- Run `cargo fmt` & `cargo clippy` before PRs

## Security Notes

- Rate limiting on all public endpoints
- CORS restricted to frontend origin in production
- No secret keys logged or returned in responses
- Use secure signing flow (env for dev, hardware/HSM for prod)

## License

MIT License

Happy building, Kelechi!  
If you want to add Docker support, database integration (e.g., sqlx + Postgres), or more detailed endpoint examples, just let me know and I’ll expand it. 🚀
