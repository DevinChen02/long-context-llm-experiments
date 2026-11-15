# Sources and attribution

This repository consolidates the following source snapshots. All files tracked at these commits were copied without content changes; Git metadata was excluded. Each experiment keeps its original filenames, relative paths, README, saved outputs, and license files.

| Experiment | Source | Commit | Files | License file in snapshot |
| --- | --- | --- | --- | --- |
| Context length sweep | [VictorHe404/Sweep-Context-Length-](https://github.com/VictorHe404/Sweep-Context-Length-) | [`573c986aa6a5`](https://github.com/VictorHe404/Sweep-Context-Length-/commit/573c986aa6a518600efda5acf4eff845552cd77f) | 2 | No license file present |
| Needle position perturbation | [DevinChen02/needle_position_perturbation](https://github.com/DevinChen02/needle_position_perturbation) | [`945a8d09948d`](https://github.com/DevinChen02/needle_position_perturbation/commit/945a8d09948d43f065546741f2e34ce5332a7d12) | 15 | No license file present |
| Prompting variants | [AXF-COCO/longbench-v2-api](https://github.com/AXF-COCO/longbench-v2-api) | [`84ac801e4be2`](https://github.com/AXF-COCO/longbench-v2-api/commit/84ac801e4be23083610dd9cf96e797aa36301ecd) | 21 | MIT; retained in `experiments/prompting_variants/LICENSE` |

## Import record

[import-manifest.json](import-manifest.json) records each destination and the SHA-256 hash of every imported file. It can be used to distinguish source content from added documentation and dependency lists. No shared root license is assigned to the combined project.

## Benchmark attribution

The experiments build on LongBench v2; the needle experiment also uses QuALITY. Dataset downloads remain in the original scripts and notebooks.

- [LongBench v2 dataset](https://huggingface.co/datasets/zai-org/LongBench-v2) and [paper](https://arxiv.org/abs/2412.15204).
- [QuALITY dataset and source](https://github.com/nyu-mll/quality).
- The original LongBench citations and MIT notice remain in the [prompting experiment README](../experiments/prompting_variants/README.md) and [LICENSE](../experiments/prompting_variants/LICENSE).
