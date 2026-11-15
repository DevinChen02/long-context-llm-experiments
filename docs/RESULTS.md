# Saved experiment results

These results were imported from the source repositories. Consolidation did not run new model inference. The experiments use different models and protocols, so their accuracy values should be interpreted within each experiment.

## Context length sweep

The notebook contains saved output for 720 calls (120 questions at six target lengths) using `qwen3-vl-235b-a22b-instruct`. The raw sweep CSV is not included upstream.

| Approximate target tokens | Saved accuracy | Mean latency |
| --- | --- | --- |
| 2,000 | 32.5% | 1.7 s |
| 4,000 | 32.5% | 1.4 s |
| 8,000 | 32.5% | 1.9 s |
| 16,000 | 46.7% | 3.3 s |
| 32,000 | 51.7% | 7.9 s |
| 64,000 | 47.5% | 23.0 s |

Accuracy is not monotonic in context length. Longer contexts also increase latency, especially at the largest budget. The token approximation, evidence truncation, and sampling-index issue described in [RUNNING.md](RUNNING.md) limit causal interpretation.

## Needle position perturbation

Each saved model summary reports 157 questions and 471 calls across three positions. Values below are copied from `model_stats_*.json`; API errors are shown because they can affect accuracy. Answer correctness is used as a retrieval-success proxy, so failures can also reflect reasoning or output-format errors.

| Model | 10% position | 50% position | 90% position | Overall accuracy | API errors |
| --- | --- | --- | --- | --- | --- |
| claude-opus-4-7 | 86.0% | 87.9% | 88.5% | 87.5% | 6 |
| deepseek-v3 | 74.5% | 79.6% | 72.6% | 75.6% | 0 |
| gemini-3.1-flash-lite-preview | 79.0% | 81.5% | 80.2% | 80.2% | 0 |
| gpt-5.4-nano | 60.5% | 58.0% | 58.6% | 59.0% | 0 |
| qwen3-vl-flash | 55.4% | 52.9% | 56.0% | 54.8% | 25 |

Position effects vary by model: the middle has the highest accuracy for DeepSeek and Gemini, but the lowest for GPT-5.4 nano and Qwen in these saved runs. [Comparison figures](../experiments/needle_position/figures/) also show context-length, subdomain, and latency results.

## Prompting variants

The six JSONL files contain 503 predictions each, with matching question IDs across variants and models. The table is computed from `judge` (accuracy) and `follow_instruction` (format compliance) in those files. The offline `result.py` script also produces breakdowns by difficulty and length.

| Model | Direct accuracy | Evidence-first accuracy | Direct format failure | Evidence-first format failure |
| --- | --- | --- | --- | --- |
| gemini-2.5-pro | 57.5% | 41.0% | 3.6% | 9.9% |
| gpt-5-mini | 45.1% | 39.0% | 0.2% | 0.0% |
| mimo-v2-omni | 37.0% | 37.6% | 18.3% | 0.2% |

Direct prompting has higher accuracy for Gemini and GPT-5 mini. MiMo has similar accuracy under both strategies, with fewer format failures under evidence-first prompting. Evidence extraction introduces a second model call; the saved results do not establish that it improves cost or latency.

## Interpretation limits

These are descriptive results from the imported configurations. They do not establish statistical significance, equal compute budgets across prompting strategies, or general rankings across model families. Model identifiers are retained as recorded in the sources; current API availability was not verified.
