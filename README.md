# KofLang model-training experiments

This repository records adapting CEIA-BR Lumi-4B for KofLang 0.6.10 coding through Pi's local tool workflow. **Lumi training stopped at V38 as requested; V17 remains the selected default and V38 was not promoted.**

## Result at the stopping point

V37 and V38 scored **2/7 each** on the same controlled V5 slice, with identical per-case outcomes. Both passed the derived-task continuation and Kof Cloud hard-cap cases; five code-generation cases failed. V38 also scored **0/1** on the typed-function holdout. No full suite or larger-project gate was run for V38 because it did not improve the discriminator.

V38 training used 649 rows / 145,602 tokens on an Intel Arc A770 (16 GB, XPU/BF16), 649 steps, one epoch, and 256-token sequences. Runtime was 2,900.48 s; mean train loss was 0.39965. Training loss is not a coding-reliability score.

See [TRAINING_HISTORY.md](TRAINING_HISTORY.md) for lineage, gates, failures, and hashes; [TRAINING.md](TRAINING.md) for the reusable Unsloth CLI profile; [PUBLICATION_SCOPE.md](PUBLICATION_SCOPE.md) for what is deliberately not redistributed.

## Next model candidate

Qwen3-4B-Instruct-2507 is now an inference-tested candidate, not a trained one. The Apache-2.0 base was served from a Q4_K_M GGUF (SHA-256 `2fde00ce69dd4899c70d020845e2638353015bba0fdf161b3eb965f2bca4464e`) through llama.cpp/Vulkan and Pi on Arc 140T. It scored **1/7** on V5 (only derived-task continuation passed) and **0/1** on the typed-function holdout. Pi tool calls worked; Kof generation and stopping behavior remain unreliable for daily use. One warmed 88-token completion measured **15.13 tok/s**. No Qwen adapter was trained. [Per-case results, hashes and limits](reports/qwen3-4b-arc-140t-pi-kof-baseline.json).

The V37/V38 **2/7** results used a V39 ablation system prompt; Qwen used the default prompt, so the scores are not a controlled head-to-head comparison.

Xenolith targets Gemma 4 26B-A4B QAT, not Qwen; the model is 26B total parameters (3.8B active) and its GGUF is 13.27 GiB, so it is not the small trainable candidate sought here. A local smoke on Arc 140T passed Xenolith's GPU checks and both XenoPi real-model MCP/codemode acceptance tests (2/2). One-sample Xenolith measurements at context 4096 were 151.68 tok/s prefill (128 tokens) and 18.29 tok/s decode (32 tokens). The dual-GPU host needed a local-only Level Zero driver-selection patch; no upstream change was made. The MCP tests exercised an echo fixture, not Kof coding quality. Each service start spent about 232 seconds hashing the model on the external test drive, so the throughput figures exclude substantial cold-start cost. Qwen compatibility with Xenolith remains unestablished; see [PUBLICATION_SCOPE.md](PUBLICATION_SCOPE.md) for the inference caveat.

## Repository contents and license

The repository includes two source-derived Kof seed corpora and the final V38 SFT corpus under [`datasets/`](datasets/README.md), distributed under GPL-3.0-only with a separate license copy. The mixed-source corpora, base-model weights, tokenizer files, LoRA adapters, Kof source/compiler bundle, and raw evaluation traces remain excluded. Lumi's license is undeclared, and mixed-corpus per-row rights are unresolved; no GPL or MIT license is claimed for those artifacts.

The [LICENSE](LICENSE) applies only to the original documentation and generic YAML profile present here. See [PUBLICATION_SCOPE.md](PUBLICATION_SCOPE.md) before reusing any model/data references.
