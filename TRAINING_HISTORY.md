# Training history and final evaluation

## Scope

Goal: improve Lumi-4B on KofLang 0.6.10 coding through Pi tools, especially conditional sums/counts, list filtering, typed functions, `switch` in loops, and continuing a project workflow after exploring files. The work produced successive LoRA adapters and increasingly tool-oriented evaluation. The benchmark remained small; a reliable full-project coding result was not established.

Training hardware: Intel Arc A770, 16 GB, Intel XPU, BF16. Unsloth training used LoRA/4-bit loading. The proven memory-safe context was 256 tokens; a 512-token V38 attempt OOMed on the A770 before the successful 256-token run.

## Adapter lineage and decisions

- **V1–V18:** early Kof SFT and Pi-tool adapters. V17 became the selected default; it scored **7/9** on corrected V2.1. V18 improved the older V2 result from 4/9 to 5/9 but did not improve the fresh V3 tasks over V17 (both 1/2).
- **V19–V24:** branches, not a linear chain. V19 OOMed at 512 tokens after 5/66 planned steps. V20 branched from V18 (**7/9, 2/2, 4/11** on V2/V3/V4); V21 continued from V20, but its saved V2 score **0/9** is invalid as a comparison because the first two tasks timed out before tool use. V22 branched from V20 (**8/9, 2/2, 4/11**); V23 branched from V21 (**8/9, 2/2, 5/11**); V24 continued from V23 (**8/9, 2/2, 5/11** on corrected V2.1/V3/V4).
- **V25–V26:** both trained from V24. Corrected V25 and V26 results tied V24 at **8/9, 2/2, 5/11**; neither passed the project gate or was promoted.
- **V27–V33:** V27 and V30 OOMed at longer sequence lengths; V31 was cancelled for data-quality defects. V28 scored **0/1** typed and **0/6** targeted V4; V29 **0/7**; V32 **0/1** typed and **0/6** targeted V5. V33's retained log records 52 steps and mean loss 1.14355; its results scored **0/1** typed and **0/6** targeted V5. V33 has results/logs but no normalized run entry in the provenance register.
- **V34–V35:** positive-only SFT improved the full V5 result from **2/12 to 3/12**, but the shared six-case slice stayed **1/6**. V34's KOOKIE attempt failed before edits; V35's real-project run failed on malformed tool-call output before edits.
- **V36:** retained prompt/evaluation controls are not directly comparable across prompt variants: one V5 slice scored **5/8** under an earlier prompt; the later three-case V39 prompt-variant control scored **0/3**; typed-function scored **0/1**. No normalized V36 training record was retained.
- **V37:** trained from V24 on 589 rows / 135,640 tokens, one epoch, 589 steps, mean loss 0.40015, 2,653.33 s. An earlier three-case discriminator scored **0/3**; the later matched V37/V38 run scored **2/7**. A separate hard-cap project attempt had **1 failing test of 4**. On the real KOOKIE project copy, it made four rejected edits and ran no candidate tests.
- **V38:** trained from V24 on 649 rows / 145,602 tokens, one epoch, 649 steps, mean loss 0.39965, 2,900.48 s, 50.199 tokens/s. V38 is one adapter descended from V24; it is not a weight merge of every run. Its training corpus incorporated V37 material plus additional positive examples.

## Matched V37/V38 evaluation

Both adapters used the same V5 benchmark cases and V39 ablation system prompt:

| Case | V37 | V38 |
|---|---:|---:|
| `conditional-loop-sum-even` | fail | fail |
| `conditional-loop-count` | fail | fail |
| `list-count-even` | fail | fail |
| `list-filter-range-sum` | fail | fail |
| `switch-in-while-loop` | fail | fail |
| `tool-continuation-derived-task` | pass | pass |
| `kof-cloud-backoff-hard-cap` | pass | pass |
| **Total** | **2/7** | **2/7** |

V38 also failed V2.1 `typed-function-bracket` (**0/1**): the generated function returned a value from a function Kof inferred as void (`SEM093`); Pi then timed out after `kof_list` and `kof_apply`. V37's prior typed-function run was also 0/1.

The matched V5 hard-cap case passing does not erase the separate V37 isolated hard-cap result (1/4 project tests failed). The full KOOKIE project workflow was not passed by V37; V38 did not proceed to larger project gates because it showed no targeted improvement.

## Promotion rule and disposition

The intended promotion gates were V2.1 ≥7/9, V3 2/2, V4 11/11, V5 12/12, KOOKIE regression 2/2 and full suite 0 failed of 169, all project tests passing, and a successful launcher smoke. V38 did not meet the first targeted discriminator, so full gates were not run. **V17 remains default. V38 was the final Lumi training iteration; no V39 was started.**

The results still do not demonstrate reliability for conditional counts/sums, list count/filter semantics, switch-in-loop code, new typed functions, or longer real-project edit/test cycles. Loss reduction did not establish behavioral progress.

## Artifact fingerprints (not redistributed here)

- V38 config SHA-256: `c45e4594054bb31db33da6a9c6fc3fb639fa74d666e16500ca2fbc6d1ba6426b`
- V38 corpus SHA-256: `8fb443d3718abe3bce6164531d1c6ac6f941f5f910477411954688025bc0c8b5`
- V38 adapter weights SHA-256: `49668e98bf2e4f25dd6b2a07d029de58d489b0f35f2ac883d314f39ea1121552`
- V38 training log SHA-256: `21642f5d8b7d386d9013f0b44396289f958383802a049ff89d19a38bcd70a459`
- V37 controlled V5 result SHA-256: `e70ef33e26a53c79b75295d0575bf783cd7222f74075c8b3426efb2877cceb8e`
- V38 controlled V5 result SHA-256: `375431072241add03b1ed0b40d1f44b1abdd0d8c6808b865ee6dd11babf73292`
- V38 typed-function result SHA-256: `4cef9c8116875ca80f760eaa379cfd078935ea7af6cb3e6cd6189a3357831e05`

See [PUBLICATION_SCOPE.md](PUBLICATION_SCOPE.md): hashes are provenance identifiers, not download links or license grants.
