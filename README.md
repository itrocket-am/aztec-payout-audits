# ITRocket Aztec payout audits

Public audit records of ITRocket's off-chain delegator payouts on Aztec mainnet.

## Details

| | |
|---|---|
| Provider | [ITRocket, provider 70](https://stake.aztec.network/providers/70) |
| Distribution wallet | [`0xBf57b4E7D3cbE8c292f6d29BDAC1070D4F0AFD27`](https://etherscan.io/address/0xBf57b4E7D3cbE8c292f6d29BDAC1070D4F0AFD27) |
| Commission | **25%**, matching our on-chain take rate |
| Cadence | Weekly, on Mondays |
| Tool | [aztec-staking-payout](https://github.com/AztecProtocol/aztec-staking-payout) |

## How it works

The commission is fixed in each delegation's split contract at the moment of staking. For delegations made before the 25% rate, our sequencers send rewards to the distribution wallet since 19 Nov 2026, and every week each delegator receives **75%** of what their own validators earned.

Delegations made after the rate change already have 25% in their split and are paid on-chain as usual.

## What's in `runs/`

One file per payout, `epoch-<from>-<to>-<runId>.json`, with the epoch range, every counted checkpoint and its L1 transaction, the per-delegator transfers and the payout transaction hashes.

## Verify a payout

1. Open the file for the period: `transfers` shows what each delegator received.
2. Pick any checkpoint in `attributedCheckpoints` and open its `txHash` on Etherscan: it was proposed by that delegator's validator.
3. Or re-run the tool for the same epoch range. The result is deterministic and must match exactly.

Background: [Operator commission adjustments](https://forum.aztec.network/t/operator-commission-adjustments/8588) on the Aztec forum.
