# ITRocket Aztec payout audits

Audit records of off-chain delegator payouts by ITRocket, Aztec staking provider 70.

- Provider page: https://stake.aztec.network/providers/70
- Distribution wallet: [`0xBf57b4E7D3cbE8c292f6d29BDAC1070D4F0AFD27`](https://etherscan.io/address/0xBf57b4E7D3cbE8c292f6d29BDAC1070D4F0AFD27)
- Commission: 25%
- Cadence: weekly
- Tool: [aztec-staking-payout](https://github.com/AztecProtocol/aztec-staking-payout)

Each payout adds `runs/epoch-<from>-<to>-<runId>.json` with the counted checkpoints, per-delegator transfers and transaction hashes. To verify, re-run the tool for the same epoch range; the result is deterministic.
