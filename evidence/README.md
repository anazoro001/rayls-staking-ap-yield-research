# Evidence index

The images below were supplied during the research conversation. They support the stated observations but have different strengths. See the main README for scope and caveats.

| File | What it shows | Caveat |
|---|---|---|
| `dashboard_oct_11.png` | Full Active Validators dashboard supplied by researcher | The webpage does not print the capture date; date is based on researcher-provided context. |
| `dashboard_aug31_comparison_excerpt.png` | Cropped excerpt from the researcher's comparison graphic, labelled 31 Aug 2026, with the older dashboard embedded | Reduced-resolution derived image, not the original full-resolution historical screenshot. The detailed values in the table are transcribed from researcher-provided comparison material. |
| `dashboard_oct11_comparison_excerpt.png` | Cropped excerpt from the researcher's comparison graphic labelled 11 Oct 2026 | For the full readable Oct 11 table, use `dashboard_oct_11.png`. |
| `issue_103.png` | GitHub Issue #103 page | Use the linked live GitHub page for current status and complete context. |
| `issue_178.png` | GitHub Issue #178 page | The issue reports devnet observations; it is not an independent mainnet test. |
| `pr_117.png` | GitHub PR #117 page | A merge does not establish deployment or activation of the feature. |
| `reward_distributor_open_tier_read.png` | Simulated `0x05cdbe8c` fallback call returning `0x708` | Selector maps to `openTierTargetApyBps()`; the named ABI was not exposed by the proxy interface. |
| `fee_aggregator_implementation.png` | Explorer page naming `FeeAggregator`, path `src/fees/FeeAggregator.sol` | Explorer banner says partial source-code match. |
| `reward_distributor_proxy.png` | `e5` proxy Read/Write interface exposing generic fallback | Does not itself identify the implementation name. |
| `delegation_pool_proxy.png` | DelegationPool proxy interface/source context | The successful return is transcribed in the main README; this image is not a full standalone result capture. |

## Read results transcribed into README

The following results were supplied in chat as explorer output text; there is no separate full-resolution screenshot of each raw output in this evidence folder:

- `getTotalDelegatedStake(Validator 3)` raw output: `0x0000000000000000000000000000000000000000004e0208db383763f2290000`
- `getBalanceBreakdown(Validator 3)`: `5000000000000000000000000` and `5000000000000000000000000`
- `targetApyBps()` selector `0x8265ea06`: returned `0x...0708`
- `openTierTargetApyBps()` selector `0x05cdbe8c`: returned `0x...0708`

If original, full-resolution contract-result screenshots are available, add them here and update this index.
