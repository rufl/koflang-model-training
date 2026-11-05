# KofLang model-training experiments

A public research record of adapting CEIA-BR Lumi-4B for KofLang 0.6.10 coding tasks through Pi's local tool workflow. The experiment ended at V38 as requested. **V17 remains the selected default; V38 was not promoted.**

## Result at the stopping point

V37 and V38 scored **2/7 each** on the same controlled V5 slice, with identical per-case outcomes. Both passed the derived-task continuation and Kof Cloud hard-cap cases; five code-generation cases failed. V38 also scored **0/1** on the typed-function holdout. No full suite or larger-project gate was run for V38 because it did not improve the discriminator.

V38 training used 649 rows / 145,602 tokens on an Intel Arc A770 (16 GB, XPU/BF16), 649 steps, one epoch, and 256-token sequences. Runtime was 2,900.48 s; mean train loss was 0.39965. Training loss is not a coding-reliability score.

See [TRAINING_HISTORY.md](TRAINING_HISTORY.md) for lineage, gates, failures, and hashes; [TRAINING.md](TRAINING.md) for the reusable Unsloth CLI profile; [PUBLICATION_SCOPE.md](PUBLICATION_SCOPE.md) for what is deliberately not redistributed.

## Next model candidate

The next low-risk cross-model candidate is [Qwen/Qwen3-4B-Instruct-2507](https://huggingface.co/Qwen/Qwen3-4B-Instruct-2507). Its official card reports 4B parameters, Apache-2.0, coding results, tool-use support, and BFCL-v3 61.9. Those are upstream results, **not** a Pi/Kof evaluation. No Qwen checkpoint was downloaded or trained here. Pi tool-call compatibility and Arc 140T speed remain untested.

Xenolith reports Arc 140T support, but its README targets a specific Gemma 4 26B-A4B QAT checkpoint; Qwen support is not established. See [PUBLICATION_SCOPE.md](PUBLICATION_SCOPE.md) for the inference caveat.

## Repository contents and license

This repository contains only the experiment documentation and a generic Unsloth profile. The training corpora, embedded training examples, base-model weights, LoRA adapters, Kof source/compiler bundle, and raw evaluation traces are not included while redistribution rights remain unresolved. No MIT license is claimed for those artifacts.

The [LICENSE](LICENSE) applies only to the original documentation and generic YAML profile present here. See [PUBLICATION_SCOPE.md](PUBLICATION_SCOPE.md) before reusing any model/data references.
