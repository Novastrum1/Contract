# Production governance policy

This policy is a launch gate, not an authorization to deploy, upgrade, move funds, or offer a service. It applies before any VELTRIXAE Mainnet contract or liquidity operation.

## Legal and compliance

No launch jurisdiction is approved yet. Until qualified counsel gives written advice for the intended markets, VELTRIXAE must not market a Mainnet product, accept customer funds, or state that it is compliant or licensed.

Before launch, an authorized project representative must retain counsel and record the intended jurisdictions, entity and compliance contact, applicable sanctions/AML/consumer-disclosure/tax/privacy obligations, and the review date.

## Independent audit

Every Mainnet contract version requires an independent Soroban security audit covering Factory, Pair, Router, privileged paths, upgrade and pause controls, arithmetic, authorization, integration assumptions, and the exact compiler/optimizer artifacts.

The release record must contain the signed scope, final report, findings and remediations, auditor attestation for the exact commit/WASM hashes, and maintainer acceptance of residual risks. A prior audit of upstream Soroswap is not an audit of VELTRIXAE changes.

## Governance defaults

Use organization-controlled Stellar multisig accounts, never a single private key, for every privileged address.

| Function | Default control | Required action |
| --- | --- | --- |
| Contract admin / upgrade authority | 3-of-5 multisig | 3 approvals, public 48-hour notice, and passing release checks |
| Emergency pause guardian | Separate 2-of-3 multisig | Pause only; cannot upgrade, withdraw, or change fees |
| Treasury / deployment account | Separate 3-of-5 multisig | Per-transaction limits and independently reconciled ledger records |
| CI / deploy automation | No signing authority | May prepare artifacts and verification reports only |

The five admin signers should be independent people in engineering, security, operations, finance, and an external/independent role. No signer device, recovery material, or approval channel may be shared. Rotate a signer only via the same threshold and record the change publicly.

## Custody and funds policy

VELTRIXAE is non-custodial: user wallets sign user transactions directly. The backend and frontend must never receive private keys, recovery phrases, signed transaction envelopes, or user custody rights. Treasury funds and user liquidity are separate; neither may fund the other.

## Mainnet release gates

All gates must be green before a human with the required threshold may submit a Mainnet transaction:

1. Counsel has approved the intended jurisdictions and public disclosures.
2. The independent audit is complete and all critical/high findings are fixed or explicitly accepted by the governance threshold.
3. The exact source commit, lockfiles, compiler, optimizer, bindings, and WASM hashes are reproducible and independently verified.
4. Testnet rehearsal proves deployment, initialization, pause, recovery, monitoring, and rollback/containment procedures without secrets in CI.
5. Mainnet signer accounts, thresholds, recovery procedures, monitoring, incident contacts, and funding limits have been independently reviewed.
6. A public deployment manifest and post-deployment verification report are prepared before the first user-facing announcement.

No document, script, CI job, or environment variable may bypass these gates.
