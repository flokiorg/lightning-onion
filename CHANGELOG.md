# Changelog

## [1.0.3]

### Changed

- Built with Go 1.26.5. (#3)

## [1.0.2-alpha]

### Fixes

- **Standalone build break**: `go.mod` had `replace github.com/flokiorg/go-flokicoin => ../go-flokicoin`, which only resolves inside the org's shared `go.work` layout ("replacement directory ../go-flokicoin does not exist" on a normal checkout). The pinned version (`v0.25.13-alpha`) is a real, resolvable release, so the `replace` directive was removed and `go mod tidy` run — no code changes needed.

### CI

- Added `.github/workflows/ci.yaml`: runs `go build`, `go vet`, and `go test` on push to `main` and on pull requests.

## [1.0.1-alpha]

### Dependency Updates

- **go-flokicoin**: Updated to `v0.25.13-alpha` for MuSig2 support and TestNet4 port fixes.
- Routine `go mod tidy` cleanup.

## [1.0.0-alpha]

### Initial Release — Flokicoin Onion Router

This is the first release of `lightning-onion` for the Flokicoin ecosystem, forked from [`lightningnetwork/lightning-onion`](https://github.com/lightningnetwork/lightning-onion) and adapted to use native Flokicoin Go packages.

### What This Library Provides

- **Sphinx onion packet construction and processing** (BOLT #04 compliant)
- **Route blinding** — build and process blinded payment paths
- **Onion message routing** — jumbo onion packets up to 32768 bytes
- **Error encryption/decryption** — backward onion error propagation
- **Replay protection** — in-memory and no-op replay log implementations
- **ECDH key derivation** — shared secret generation per hop using secp256k1

This library is a pure cryptographic routing layer with no dependency on chain parameters, ports, or network topology. It is consumed directly by `flnd` as the onion routing engine for the Flokicoin Lightning Network.

### Changes from Upstream

#### Module

| | Path |
|--|--|
| **Before** | `github.com/lightningnetwork/lightning-onion` |
| **After** | `github.com/flokiorg/lightning-onion` |

#### Dependency Migration

| Replaced | With |
|--|--|
| `github.com/btcsuite/btcd/btcec/v2` | `github.com/flokiorg/go-flokicoin/crypto` |
| `github.com/btcsuite/btcd/wire` | `github.com/flokiorg/go-flokicoin/wire` |

The `go-flokicoin/crypto` package is a type-alias wrapper over `decred/dcrd/dcrec/secp256k1/v4` — the same underlying elliptic curve library — so all cryptographic behaviour is identical to upstream.

#### API Compatibility

All exported types and functions are unchanged. Consumers updating from the upstream module only need to change the import path.
