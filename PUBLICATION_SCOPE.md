# Publication scope and licensing status

This public repository is an archival summary and a generic Unsloth profile. It does **not** redistribute the full local training workspace.

## Not included

- Lumi base-model weights, tokenizer files, or V17/V24/V37/V38 adapters.
- Kof-derived training JSONL, data-builder scripts containing embedded examples, Kof source/compiler/SDK bundles, benchmark definitions, or raw Pi/evaluation traces.
- Local caches, logs containing full prompts/output, private upload files, or temporary workspaces.

These exclusions are deliberate, not missing downloads. The local provenance record says the CEIA-BR `lumi-4b` model card does not declare a license. It also records training material derived from Kof 0.6.10 source at revision `b5b63d14eca5ac6ab0ba80fd7c8ea00b03f387c0`, licensed GPL-3.0. The combined corpus contains additional generated/curated material, and its complete redistribution terms were not resolved. The adapter therefore has **no declared MIT license**. No model or dataset license is asserted here.

The GPL-3.0 status of the upstream Kof source does not itself resolve the license of the combined corpus or model adapter. The user-requested MIT intent was not treated as proof of redistribution rights.

## License boundary

The root [LICENSE](LICENSE) applies only to the original documentation and the generic YAML profile in this repository. It does not license CEIA-BR Lumi, Kof source or binaries, any training data, any adapter/checkpoint, or any other upstream material. The Qwen candidate is referenced only; no Qwen weights are included.

## Evidence retained

[TRAINING_HISTORY.md](TRAINING_HISTORY.md) records aggregate scores, run metrics, and SHA-256 fingerprints for withheld artifacts. A hash is not a copy of an artifact and grants no redistribution right. Raw result traces and local absolute paths were omitted from this public copy.

Before publishing training corpora, raw traces, or adapter weights, resolve the base model's license and audit every dataset/source license and provenance. Do not label the adapter MIT based solely on the intended project license.
