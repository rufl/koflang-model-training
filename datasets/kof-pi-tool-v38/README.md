# V38 Pi-tool SFT corpus

`kof-0.6.10-pi-tool-v38.jsonl` is the final 649-row conversational corpus used for Lumi V38. It contains compiler/runtime-verified Kof examples and synthetic project fixtures generated through the V34 → V36 → V37 → V38 data-builder chain. The examples and fixtures in this corpus are released under GPL-3.0-only by project choice; the full license is at [`../COPYING`](../COPYING). Kof compiler output is not automatically GPL; the license here applies to this authored dataset.

- Rows: 649
- Input tokens under the Lumi tokenizer: 145,602
- Maximum formatted row: 256 tokens
- SHA-256: `8fb443d3718abe3bce6164531d1c6ac6f941f5f910477411954688025bc0c8b5`
- Provenance: [`PROVENANCE.json`](PROVENANCE.json)

The base Lumi model, tokenizer, and LoRA weights are not included. This corpus can be adapted to another supported model, but its token lengths and training behavior must be recalculated for that tokenizer.
