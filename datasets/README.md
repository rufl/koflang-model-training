# Released training datasets

All datasets and metadata in this directory are distributed under **GPL-3.0-only**; the full license text is in [`COPYING`](COPYING). The repository-root [`LICENSE`](../LICENSE) separately applies only to the original documentation and generic Unsloth YAML profile.

## Included

- [`kof-source-derived/`](kof-source-derived/README.md): two Kof 0.6.10 seed corpora extracted from KofLang/Kof4j source and documentation at revision `b5b63d14eca5ac6ab0ba80fd7c8ea00b03f387c0`.
- [`kof-pi-tool-v38/`](kof-pi-tool-v38/README.md): the 649-row final V38 Pi-tool SFT corpus. It contains generated Kof programming tasks and synthetic project fixtures; its provenance report is included alongside it.

## Deliberate exclusions

This is **not** the complete training-data archive. The full-corpus builder records inputs from Kof GPL-3.0, an upstream site and course with `NOASSERTION`, BSD-licensed projects, and an Apache-2.0 project. The combined v11/v12 corpus does not preserve per-row licensing, so it cannot safely be relicensed wholesale as GPL. Other historical corpora are excluded until their source-level licensing can be separated and verified. See [`../PUBLICATION_SCOPE.md`](../PUBLICATION_SCOPE.md).

The GPL license for these data files does not grant rights to CEIA-BR Lumi weights, tokenizer files, LoRA adapters, or any other excluded upstream work.
