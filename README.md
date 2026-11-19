# ITRocket: Aztec delegator payout audits (provider 70)

Public audit records for off-chain delegator payouts by **ITRocket**, Aztec staking provider **70**.

## Why payouts are off-chain

Aztec bakes the provider commission into each delegation's split contract when the stake is made, and it cannot be changed afterwards. Following the Aztec forum post [Operator commission adjustments](https://forum.aztec.network/t/operator-commission-adjustments/8588), ITRocket moved its legacy delegations (staked at 0% and 5%) to the community baseline of **25%**:

- Since **v6 epoch 1504 (19 Nov 2026)** the sequencers of these delegations use the distribution wallet below as coinbase.
- Every week ITRocket pays each delegator **75%** of the rewards their own attesters earned, using [AztecProtocol/aztec-staking-payout](https://github.com/AztecProtocol/aztec-staking-payout).
- Delegations made after 11 Jun 2026 already have 25% in their split contract and are paid on-chain by the split as usual. They are not part of these payouts.

## Parameters

| | |
|---|---|
| Provider ID | 70 |
| Distribution wallet (coinbase) | [`0xBf57b4E7D3cbE8c292f6d29BDAC1070D4F0AFD27`](https://etherscan.io/address/0xBf57b4E7D3cbE8c292f6d29BDAC1070D4F0AFD27) |
| Commission | 25% (2500 bps), same as the on-chain take rate in the StakingRegistry |
| Rollup (v6) | `0xE37A2663Cc9c119BA41572C40fAb0bbb23a5402f` |
| Token | AZTEC `0xA27EC0006e59f245217Ff08CD52A7E8b169E62D2` |
| First settled epoch | 1504 |
| Cadence | weekly, on Mondays |
| Tool | `AztecProtocol/aztec-staking-payout` @ `6be79d0` |
| Attribution | `proposals`: each delegator receives 75% of the checkpoints proposed by their own attesters |

## Contents

- `runs/epoch-<from>-<to>-<runId>.json`: audit record of each payout: epoch and L1 block range, every counted checkpoint with its L1 tx hash, reward reconciliation against the rollup, per-delegator transfers, and the executed transaction hashes.
- `runs/EXECUTIONS.md`: one line per payout with the claim and payout transactions.
- `config.public.yaml`: the tool config used for every run (without the RPC URL).

## How to verify a payout

1. Open the audit JSON. `rewardEarned` must equal the sum of `attributedCheckpoints`, and `rewardReconciliation` shows it matches the rollup's own reward counter for the distribution wallet.
2. Pick any row in `attributedCheckpoints` and look up its `txHash` on Etherscan: the `propose()` signature recovers to that row's attester, and the checkpoint header coinbase is the distribution wallet.
3. For each delegator, sum their checkpoints' rewards and take 75%. The result must match `transfers[]` and the token transfers in the `txHashes` transactions.
4. Or re-run the tool yourself with `config.public.yaml`, your own archival RPC and the same `--from-epoch` / `--to-epoch`. For a fixed epoch window the result is deterministic.
