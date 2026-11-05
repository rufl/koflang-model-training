# Reusable Unsloth SFT

This repository provides a generic starter profile for Unsloth's native `unsloth train` CLI. It intentionally has no model, dataset, output path, or architecture-specific LoRA target modules. Supply a supported Hugging Face Transformers checkpoint and a conversational JSONL dataset of `messages` arrays. GGUF files are inference artifacts, not training checkpoints.

## Preflight

From the repository root, replace `/path/to/your-train.jsonl` with a dataset you have rights to use:

```bash
UNSLOTH="/path/to/unsloth/.venv/bin/unsloth"
MODEL="Qwen/Qwen3-4B-Instruct-2507"
DATA="/path/to/your-train.jsonl"
OUTPUT="$HOME/.unsloth/studio/outputs/qwen3-4b-kof-sft"

"$UNSLOTH" train \
  --config configs/unsloth-sft-portable.yaml \
  --model "$MODEL" \
  --local-dataset "$DATA" \
  --format-type conversational \
  --output-dir "$OUTPUT" \
  --dry-run
```

Review the resolved configuration and remove `--dry-run` only when ready to train. Common overrides include `--max-seq-length`, `--num-epochs`, `--learning-rate`, `--batch-size`, `--gradient-accumulation-steps`, `--lora-r`, and `--lora-alpha`. The selected checkpoint supplies the model/tokenizer configuration; leave LoRA target modules unspecified initially so Unsloth can select architecture defaults.

The profile uses 4-bit loading, LoRA rank 8, one epoch, batch size 1, and a 256-token sequence cap. These are starter values, not universal optima. The 256-token cap completed on the project's A770; a V38 512-token attempt OOMed. Increase sequence length only after a hardware smoke. `trust_remote_code` is off; enable it only for reviewed model code. A CLI dry-run resolves configuration but does not test model download, XPU kernel support, tool-call compatibility, or training completion.

For every run, record the exact base-model revision, dataset hash and license, config, trainer log, and adapter hash. Adapters from distinct base checkpoints cannot be treated as interchangeable; even adapters sharing a parent need evaluation after any weighted merge. A lower loss is not proof of better coding.

## Candidate and inference notes

Qwen3-4B-Instruct-2507 (Apache-2.0) has now been tested, without training, through Pi's OpenAI-compatible tool-call path on llama.cpp/Vulkan and Arc 140T. It scored 1/7 on the targeted V5 Kof gate and 0/1 on the typed-function holdout; tool-call transport works, but coding reliability is not daily-driver quality. One warmed 88-token completion measured 15.13 tok/s. This run used the default Kof system prompt, unlike V37/V38's V39 ablation prompt, so scores are not directly comparable. [Full results](reports/qwen3-4b-arc-140t-pi-kof-baseline.json).

Xenolith evidence here is for Gemma 4 26B-A4B QAT, not Qwen3. Qwen3 support in Xenolith remains untested; llama.cpp Vulkan is the verified Qwen inference path. Qwen3.5-4B is newer, but Unsloth's guide discourages 4-bit QLoRA for that family; its XPU training and Arc 140T inference remain unverified.

Sources: [Qwen3-4B model card](https://huggingface.co/Qwen/Qwen3-4B-Instruct-2507), [Unsloth Qwen3 guide](https://unsloth.ai/docs/models/tutorials/qwen3-how-to-run-and-fine-tune), [Unsloth Qwen3.5 guide](https://unsloth.ai/docs/models/qwen3.5/fine-tune), [Xenolith](https://github.com/simoneiacomino/xenolith), [XenoPi](https://github.com/simoneiacomino/xenopi).
