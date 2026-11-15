# Long-context LLM experiments

How does an LLM's ability to use long documents change with context length, evidence position, and prompting strategy? This project brings together three experiments using LongBench v2 and QuALITY, with notebooks, evaluation scripts, saved predictions, and figures.

[Read the project report (PDF)](docs/long-context-llm-report.pdf)

## Experiments

| Experiment | What it tests | Code and results |
| --- | --- | --- |
| Context length sweep | Accuracy and latency at approximate context budgets from 2K to 64K tokens | [Notebook and instructions](experiments/context_length/) |
| Needle position perturbation | Answering questions about evidence inserted at 10%, 50%, or 90% of a long document, across five models | [Notebooks, model statistics, and figures](experiments/needle_position/) |
| Prompting variants | Answering directly versus extracting evidence before answering, across three models | [API scripts, prompts, and predictions](experiments/prompting_variants/) |

## Findings from the saved results

- Context length: the saved sweep shows non-monotonic accuracy and increasing latency at larger context budgets. Longer input alone does not consistently improve answers.
- Evidence position: sensitivity to position differs by model; the middle is not uniformly the weakest position.
- Prompting: direct answers outperform evidence-first prompting for Gemini 2.5 Pro (57.5% vs. 41.0%) and GPT-5 mini (45.1% vs. 39.0%). MiMo v2 Omni is similar under both strategies (37.0% vs. 37.6%).

These are imported experiment results, not new benchmark runs. See [result details and limitations](docs/RESULTS.md) for sample counts and interpretation.

## Getting started

Use Python 3.10+ and run each experiment from its own folder. Each has separate dependencies and API configuration. For example:

```bash
cd experiments/context_length
python -m pip install -r requirements.txt
jupyter notebook context_length_sweep.ipynb
```

Set the endpoint, model, and API key in the notebook before running inference. See [setup and run instructions](docs/RUNNING.md) for all three experiments, including offline evaluation of saved predictions.

## Sources and attribution

All 38 tracked files from the three source snapshots are retained unchanged under `experiments/`, including their READMEs and the prompting repository's MIT license. New files provide navigation, setup guidance, and dependency lists. [Source repositories, commit IDs, and license information](docs/SOURCES.md) document the import.
