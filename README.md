# ipstake — Aztec staking payout audits

Public audit records for [ipstake](https://github.com/ipstake)'s off-chain delegator payouts on Aztec mainnet, produced by [aztec-staking-payout](https://github.com/AztecProtocol/aztec-staking-payout).

## Operator details

| | |
|---|---|
| Provider ID | **51** |
| Distribution wallet (L2 coinbase) | [`0xb2f3aed8eF84ad40484fC71ebb1301B8c8B4785f`](https://etherscan.io/address/0xb2f3aed8eF84ad40484fC71ebb1301B8c8B4785f) (Safe) |
| Provider admin | `0xe6b5A31d8bb53D2C769864aC137fe25F4989f1fd` |
| Commission | **25%** (2500 bips), matching our on-chain take rate on the [StakingRegistry](https://etherscan.io/address/0x042dF8f42790d6943F41C25C2132400fd727f452) |
| Cadence | Monthly |
| Reward token | [`AZTEC`](https://etherscan.io/address/0xA27EC0006e59f245217Ff08CD52A7E8b169E62D2) |

## What's in here

Each monthly settlement produces two files under [`runs/`](./runs):

- `epoch-<from>-<to>-<runId>.json` — the canonical audit record: epoch/block window, reward-config snapshot, per-attester checkpoint counts with L1 tx hashes, commission, per-delegator transfer breakdown, and the encoded calldata.
- `epoch-<from>-<to>-<runId>.safe.json` — the same transactions in Safe Transaction Builder import format.

## Verify a distribution yourself

1. Open a run's audit JSON. `checkpointsProposed × sequencerRewardPerCheckpoint` (from the `rewardConfig` snapshot) gives the total reward; `transfers[]` is the per-delegator breakdown.
2. Pick any row in `attributedCheckpoints[]` and look up its `txHash` on a block explorer — the `propose()` signature recovers to that row's attester.
3. Sum proposals per delegator, apply the 25% commission, compare to `transfers[]`. Numbers must match exactly.
4. Or re-run [aztec-staking-payout](https://github.com/AztecProtocol/aztec-staking-payout) with [`config.public.yaml`](./config.public.yaml) and the run's pinned `--from-epoch`/`--to-epoch` — results are deterministic and should be byte-identical.

Context: [Operator commission adjustments](https://forum.aztec.network/t/operator-commission-adjustments/8588) on the Aztec forum.
