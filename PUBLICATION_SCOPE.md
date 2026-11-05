# Publication scope and licensing status

This public repository contains the experiment record, a generic Unsloth profile, the V39 Kof evaluation prompt, one sanitized Qwen inference-baseline report, and only the three datasets listed under [`datasets/`](datasets/README.md). It does **not** redistribute the complete local training workspace or all historical corpora.

## Not included

- CEIA-BR Lumi base-model weights, tokenizer files, or V17/V24/V37/V38 adapters.
- The mixed-source full-corpus v11/v12 datasets, other historical Pi/tool datasets without row-level source licensing, builder scripts containing embedded examples, Kof source/compiler/SDK bundles, benchmark definitions, or raw Pi/evaluation traces.
- Local caches, logs containing full prompts/output, private upload files, and temporary workspaces.

The included datasets are scoped separately: the two seed corpora are derived from Kof 0.6.10 source and documentation at revision `b5b63d14eca5ac6ab0ba80fd7c8ea00b03f387c0`; the V38 corpus contains generated Kof tasks and synthetic project fixtures. They are released under GPL-3.0-only with the full text in [`datasets/COPYING`](datasets/COPYING). Kof's source license does not automatically license every program written in Kof; the V38 dataset is placed under GPL by project choice.

The training provenance for the mixed full corpus includes Kof GPL-3.0, site/course sources with `NOASSERTION`, BSD-licensed repositories, and an Apache-2.0 repository. The combined rows do not retain sufficient per-example licensing to split safely. Those corpora and descendants with unresolved source lineage are not published under a blanket GPL.

CEIA-BR's [`lumi-4b` model card](https://huggingface.co/ceia-br/lumi-4b) does not declare a license or full model provenance. Hugging Face currently lists the repository as public and ungated (`gated: false`); the model files are downloadable without approval. That public access does **not** establish permission to mirror or redistribute them. The adapter therefore has **no declared license**. A Kof GPL license cannot grant rights to Lumi weights or adapter derivatives; redistribution requires an upstream license or written permission from CEIA-BR.
At the checked repository revision `1c8928e189f0fb1380f5f1dab1b2e0a0fb814c6e`, Hugging Face listed approximately 7.96 GB of stored model files and no `license` metadata or `LICENSE` file. This is a metadata record, not authorization to republish the weights, tokenizer, or adapter tensors.

## License boundary

The root [LICENSE](LICENSE) covers original documentation, the generic YAML profile, [`configs/kof-system-prompt-v39-ablation.txt`](configs/kof-system-prompt-v39-ablation.txt), and [`reports/qwen3-4b-arc-140t-pi-kof-baseline.json`](reports/qwen3-4b-arc-140t-pi-kof-baseline.json). [`datasets/COPYING`](datasets/COPYING) separately applies to included datasets. Neither license applies to Lumi weights, adapters, excluded source data, or third-party model artifacts. The Qwen GGUF and all model weights are not included.

## Evidence retained

[`TRAINING_HISTORY.md`](TRAINING_HISTORY.md) records training lineage and aggregate scores. The Qwen report contains sanitized per-case results and hashes, not raw traces. [`datasets/`](datasets/README.md) contains exact released data and corpus hashes. Hashes for withheld artifacts are provenance identifiers, not download links or license grants.

To publish the remaining mixed corpora, audit and preserve per-example source/license provenance. To publish Lumi weights or adapters, obtain an explicit redistribution license or written permission from CEIA-BR. Do not relicense third-party model artifacts under GPL solely because the Kof training material is GPL.
