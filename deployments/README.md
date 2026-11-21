# Deployments

`deploy-testnet.sh` writes `<network>.json` here **after** an authorized
deployment, with the real contract IDs, WASM hashes and transaction hashes
returned by the network. `create-pool.sh` appends created pools.

No deployment record is present in the available repository/history, so there
is no `testnet.json`. This does not establish that no deployment exists elsewhere. Do not hand-edit or invent IDs: every value in these files must
come from an actual network response.

The older deployment from source commit `133b18d00505108c26382cb44045abab48b51882`
is recorded in the backend repository, with fresh read-only verification linked
from [TESTNET_VERIFICATION.md](../docs/TESTNET_VERIFICATION.md). Those legacy
records must not be copied to this operational directory: they describe an
initialize-based deployment, not the current constructor-based scripts.

Mainnet deployments are out of scope for these scripts and are refused.
