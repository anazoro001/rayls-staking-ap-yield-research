# Rayls Staking APY: Snapshot Comparison & Read-Only Contract Checks

> **Independent, point-in-time research notes — not a security audit, investment recommendation, or proof of realized yield.**

This repository documents a comparison of two researcher-supplied Rayls delegated-staking dashboard snapshots and a small set of simulated, read-only contract calls performed on **11 October 2026**. It combines observations, calculations, public-repository context, and explicit limitations so readers can reproduce or challenge the conclusions.

## Executive summary

- The dashboard snapshots are labelled **31 August 2026** and **11 October 2026**. The first snapshot's values are transcribed from the comparison material supplied by the researcher; the evidence bundle includes only a reduced-resolution crop of the older dashboard embedded in that comparison, not a separately captured full-resolution original. The 11 October screenshot shows the full validator table, but the date is supplied by the researcher rather than printed inside the page.
- Across the two supplied snapshots, delegated stake rises from **125.93M RLS** to **190.90M RLS**, a **51.6% net increase**. With seven validators each displaying 5M RLS self-stake, total displayed stake rises from **160.93M RLS** to **225.90M RLS**, a **40.4% increase**.
- A simulated read through the DelegationPool proxy for Validator 3 returned a value that, assuming 18 decimals, converts to **94,305,822.033296 RLS**. That is consistent with the dashboard's rounded **94.31M RLS** figure.
- Two calls simulated through the proxy at the address ending `e5` returned **1,800** (`0x708`) each. The selectors used match the canonical signatures `targetApyBps()` and `openTierTargetApyBps()`. **1,800 BPS = 18%**, but these stored getter values do not, by themselves, establish the effective target when a reward curve may be configured, nor do they establish realized delegator yield.
- GitHub Issue [#178](https://github.com/raylsnetwork/axyl/issues/178) reports on devnet that hybrid validator performance weights are not consumed by `RewardDistributor.distributeRewards()`. This research did not independently test that claim on mainnet.
- The exact block number used by the Simulate reads was not captured. Curve-address getters were not queried, and the deployed `e5` implementation was not independently verified against public source bytecode.

## Snapshot comparison

The values below are transcribed from the researcher's two-snapshot comparison. The first snapshot is labelled 31 August 2026; the bundle includes only a reduced-resolution crop of the older dashboard embedded in that comparison, not a separate full-resolution original. Treat the historical figures as researcher-provided evidence rather than independently reconstructed explorer history.

| Validator | Delegated stake — 31 Aug 2026 | Delegated stake — 11 Oct 2026 | Change | Self-stake on each snapshot |
|---|---:|---:|---:|---:|
| Validator 3 | 46.97M RLS | 94.31M RLS | +47.34M | 5M RLS |
| Validator 5 | 28.48M RLS | 36.56M RLS | +8.08M | 5M RLS |
| Validator 1 | 26.88M RLS | 30.53M RLS | +3.65M | 5M RLS |
| Validator 2 | 23.60M RLS | 29.50M RLS | +5.90M | 5M RLS |
| Validator 4 | 0 RLS | 0 RLS | 0 | 5M RLS |
| Validator 6 | 0 RLS | 0 RLS | 0 | 5M RLS |
| Validator 7 | 0 RLS | 0 RLS | 0 | 5M RLS |
| **Total delegated** | **125.93M RLS** | **190.90M RLS** | **+64.97M (+51.6%)** | — |
| **Total displayed stake incl. self-stake** | **160.93M RLS** | **225.90M RLS** | **+64.97M (+40.4%)** | **35M total** |

### Derived calculations

- Delegated stake change: `(190.90 − 125.93) / 125.93 = 51.6%`.
- Total displayed stake: `125.93 + (7 × 5) = 160.93M RLS`; `190.90 + (7 × 5) = 225.90M RLS`.
- Total-stake growth: `(225.90 − 160.93) / 160.93 = 40.4%`.
- Validator 3 share of net delegated increase: `(94.31 − 46.97) / (190.90 − 125.93) ≈ 72.9%`.
- Validator 3 share of 11 October delegated stake: `94.31 / 190.90 ≈ 49.4%`.
- Top three validators on 11 October: `(94.31 + 36.56 + 30.53) / 190.90 ≈ 84.5%`.
- Using Rayls' published 10B maximum RLS supply as a denominator, displayed total stake equals about **1.61%** and **2.26%** of that maximum supply respectively. This is **not** a percentage of circulating supply.

All snapshot figures are rounded to the precision shown in the dashboard. Calculations therefore inherit rounding error.

## Read-only contract observations

The calls below were run with the explorer's **Simulate** interface. No wallet was connected and no state-changing transaction was submitted. The exact block number for each simulated read was not recorded, so these results should be treated as point-in-time observations without a block-level historical anchor.

### 1. DelegationPool — Validator 3 delegated stake

- **Proxy address:** `0x07e17e17e17e17e17e17e17e17e17e17e17e17e2`
- **Function:** `getTotalDelegatedStake(address)`
- **Function selector:** `0x187f92b6`
- **Validator:** `0x90aEd90Ede55e7f784523303e12f3457d6337789`
- **Raw return value:**

```text
0x0000000000000000000000000000000000000000004e0208db383763f2290000
```

- **Decoded integer:** `94,305,822,033,296,000,000,000,000`
- **Human-readable value if RLS uses 18 decimals:** `94,305,822.033296 RLS`
- **Comparison:** rounds to `94.31M RLS`, matching the 11 October dashboard display for Validator 3.

The decimal conversion assumes 18 token decimals; the output itself is the observed raw return value. The match supports consistency between the dashboard display and this contract read, but does not supply a historical block number for the read.

### 2. ConsensusRegistry — balance breakdown

- **Proxy address:** `0x07e17e17e17e17e17e17e17e17e17e17e17e17e1`
- **Function:** `getBalanceBreakdown(address)`
- **Validator:** `0x90aEd90Ede55e7f784523303e12f3457d6337789`
- **Returned values (as provided from the explorer):**

```text
(uint256): 5000000000000000000000000
(uint256): 5000000000000000000000000
```

Each integer corresponds to **5,000,000 units** if the relevant token uses 18 decimals. The output labels did not identify the semantics of the two tuple fields, so this report does not assign them meanings beyond recording the values.

### 3. FeeAggregator identity

- The proxy address ending `e3` showed implementation address `0x07e17e17e17e17e17e17e17e17e17e17e17e17e4`.
- The implementation explorer page displayed the contract name **`FeeAggregator`** and source path `src/fees/FeeAggregator.sol`.
- The explorer banner said **partial match** for source verification; this report does not call it a complete source verification.

### 4. APY-related getters on the proxy ending `e5`

The contract page exposed a generic `fallback` interface rather than readable named functions. The following selectors were supplied to the fallback input field and simulated:

| Canonical function signature (matches public Axyl source) | Selector | Raw returned value | Decimal return | BPS-to-percent conversion |
|---|---|---|---:|---:|
| `targetApyBps()` | `0x8265ea06` | `0x0000000000000000000000000000000000000000000000000000000000000708` | 1,800 | 18% |
| `openTierTargetApyBps()` | `0x05cdbe8c` | `0x0000000000000000000000000000000000000000000000000000000000000708` | 1,800 | 18% |

`100 BPS = 1%`, so `1,800 BPS = 18%`.

**Identity/configuration caveat:** the selector names and values are consistent with the public Axyl source, but the explorer interface did not expose named getters on the `e5` proxy and this research did not independently match the deployed implementation's bytecode to `RewardDistributor.sol`. The results establish the values returned by those simulated calls, not that the underlying implementation identity is independently proven.

The public contract source describes optional `rewardCurve()` and `openTierRewardCurve()` paths that can affect the target used for distribution. Those curve-address getters were **not queried** here. Therefore, the two 18% return values must not be described as proven effective APY, a guaranteed yield, or realized delegator earnings.

## GitHub context

These references provide context. They are not substitutes for verifying the deployed contract implementation or its active configuration.

- **[Issue #103 — Replace fixed APY with a revenue-based reward curve](https://github.com/raylsnetwork/axyl/issues/103):** proposes a revenue/emission-based model. The issue's formula is context for the conditional compression calculation below, not proof that a curve was active during either dashboard snapshot.
- **[PR #85 — Open tier staking with separate APY](https://github.com/raylsnetwork/axyl/pull/85):** describes separate APY settings for the priority/whitelisted and open tiers. In public source naming, `targetApyBps()` corresponds to the non-open/priority track, while `openTierTargetApyBps()` corresponds to the open tier.
- **[PR #117 — RewardCurve POC for revenue-based staking APY](https://github.com/raylsnetwork/axyl/pull/117):** merged on 14 September 2026. The merge establishes a repository change, not proof that the feature was deployed or activated on the contract queried here.
- **[Issue #178 — RewardDistributor ignores hybrid performance weights](https://github.com/raylsnetwork/axyl/issues/178):** an open issue reporting that hybrid performance weights are not consumed during reward distribution; the issue describes devnet observations. This work did not independently test the report on mainnet.
- **[PR #179 — Wire hybrid performance weights into RewardDistributor payout (opt-in)](https://github.com/raylsnetwork/axyl/pull/179):** proposes optional wiring. This research did not establish whether it was merged, deployed, or enabled in the queried environment.
- **[Public RewardDistributor source](https://github.com/raylsnetwork/axyl/blob/main/rayls-contracts/src/fees/RewardDistributor.sol):** current public source context for static BPS getters and reward curves. The source branch is mutable and is not independently proven identical to the deployed implementation examined here.
- **[Rayls staking documentation](https://docs.rayls.com/docs/staking-on-the-rayls-public-chain):** warns that yield is not guaranteed and stake can be lost under slashing conditions.
- **[Rayls RLS token overview](https://www.rayls.com/what-is-rayls):** describes a 10 billion maximum supply. The supply comparison above uses maximum supply, not circulating supply.

## Conditional APY compression calculation

If (and only if) the starting effective APY were exactly 18%, annual emissions remained constant, and effective APY followed `annual emission / total RLS staked`, then increasing displayed total stake from 160.93M to 225.90M RLS would imply:

```text
18% × (160.93 / 225.90) ≈ 12.82%
```

This is a conditional illustration, **not a measured APY and not evidence that a reward curve was active**. Emissions could change, the dashboard rounds APY, and the active curve configuration was not queried.

## Limitations and non-claims

1. No exact block number was recorded for the Simulate reads.
2. The 11 October dashboard screenshot was supplied by the researcher and does not show the date inside the webpage; the date is based on the researcher's capture record.
3. The 31 August snapshot figures are transcribed from researcher-provided comparison material. The original full-resolution dated screenshot was not supplied independently in this evidence bundle.
4. Only Validator 3's delegated stake was queried on-chain; the other six validators' values were not individually read from contracts.
5. `getBalanceBreakdown` field semantics were not independently identified from the ABI.
6. The `e5` contract's implementation was not independently verified by source/bytecode matching.
7. `rewardCurve()` and `openTierRewardCurve()` were not queried; effective target APY and realized rewards remain unknown.
8. Issue #178's devnet observation was not independently reproduced on mainnet.
9. The optional Halborn audit review was skipped.
10. This is a technical observation log, not a formal security audit, financial advice, or recommendation to stake.

## Evidence folder

See [`evidence/README.md`](evidence/README.md) for the included images and what each one does and does not establish.

## Reproduction checklist

- Open the [Rayls staking dashboard](https://dapp.rayls.com/stake) and capture the full table with capture date recorded separately.
- Open the contracts through the [Rayls Explorer](https://explorer.rayls.com/).
- Use read/simulate functions only; do not submit state-changing transactions for reproduction of these findings.
- Record the exact timestamp, network/chain, block number if available, contract address, input calldata, raw return data, and token-decimal assumption for each read.
- Check the `rewardCurve()` and `openTierRewardCurve()` addresses before concluding that the 18% static value is the effective target.

---

*Prepared 12 October 2026 from the researcher's supplied observations and the public references linked above. Check current contract/source status independently before relying on this document.*
