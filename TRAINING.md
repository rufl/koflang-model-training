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

The first cross-model candidate is [Qwen/Qwen3-4B-Instruct-2507](https://huggingface.co/Qwen/Qwen3-4B-Instruct-2507), Apache-2.0. Qwen reports BFCL-v3 61.9 and documents coding/tool-use capabilities, Qwen-Agent, and llama.cpp support. This does not establish Pi/Kof reliability. The current Lumi server parser is model-specific; test tool-call serialization with Pi before training or relying on it.

Xenolith reports an Arc 140T test (`8086:7d51`) but targets one Gemma 4 26B-A4B QAT checkpoint. That does not establish Qwen3 support. For Qwen3, begin with llama.cpp's Intel backend; benchmark on the actual Arc 140T system before claiming performance. Qwen3.5-4B is newer, but Unsloth's guide discourages 4-bit QLoRA for that family; its XPU training and Arc 140T inference are unverified here.

Sources: [Qwen3-4B model card](https://huggingface.co/Qwen/Qwen3-4B-Instruct-2507), [Unsloth Qwen3 guide](https://unsloth.ai/docs/models/tutorials/qwen3-how-to-run-and-fine-tune), [Unsloth Qwen3.5 guide](https://unsloth.ai/docs/models/qwen3.5/fine-tune), [Xenolith](https://github.com/simoneiacomino/xenolith), [XenoPi](https://github.com/simoneiacomino/xenopi).
