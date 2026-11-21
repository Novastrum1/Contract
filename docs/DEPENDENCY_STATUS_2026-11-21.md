# Dependency status — 2026-11-21

The Contract advisory job currently fails on **RUSTSEC-2026-0387**:
`crypto-bigint 0.5.5`, patched in `>=0.7.6`. The advisory concerns timing
variability in operations intended to be constant-time. See the
[RustSec advisory](https://rustsec.org/advisories/RUSTSEC-2026-0387.html).

`cargo tree --locked --offline -i crypto-bigint` confirms the path:
`soroban-sdk 22.0.11 -> soroban-env-host 22.1.3 -> elliptic-curve 0.13.8 -> crypto-bigint 0.5.5`,
also through the host's ECDSA/k256/p256 dependencies.
`cargo tree --locked --target wasm32v1-none -i crypto-bigint` reports nothing
to print. Thus this observation places it in the local host/test dependency
graph, not the contract's WASM target graph. It does not establish that the
host package is patched, safe to handle secrets, or that all advisory checks pass.

The fixed crypto-bigint major is incompatible with elliptic-curve 0.13's
dependency constraint. A direct lockfile version edit would not be a valid
upgrade. An SDK/host/curve migration or an upstream reviewed compatible fix
must be evaluated and tested together. Until then the strict advisory job
remains failing. No advisory ignore, fail-open flag or security policy exception
has been added. Do not claim a clean dependency audit or production readiness.

## Generator repair

Merged dependency updates had left duplicate `stellar-xdr 30.0.0` entries in
`tools/spec-tool/Cargo.lock`, while the manifest still pinned the TypeScript
generator to 28.1.0. Restore a compatible parser/XDR 28.0.0 pair and regenerate
the lockfile with Rust 1.99.1. The generator compiles with `--locked`.
Dependabot groups these three coupled crates and defers major upgrades to a
reviewed migration that regenerates the checked-in clients and metadata.
This change does not alter AMM contract code or the root dependency lockfile.

Reproduce:

```sh
cargo build --locked --manifest-path tools/spec-tool/Cargo.toml
make bindings metadata
git diff --exit-code -- bindings/ metadata/
cargo audit --file Cargo.lock
cargo audit --file tools/spec-tool/Cargo.lock
```

The root audit failure above is an explicit unresolved blocker, not a successful
verification result. Build/test, generated-output comparison and advisory jobs
must be reported separately.
