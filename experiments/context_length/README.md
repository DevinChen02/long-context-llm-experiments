# Context Length Sweep Experiment

This notebook tests how a model's multiple-choice QA accuracy changes as context length grows from 2K to 64K tokens. It uses the [LongBench-v2](https://huggingface.co/datasets/zai-org/LongBench-v2) dataset and evaluates **Qwen3-VL-235B-A22B-Instruct** via a V-API endpoint.

---

## Setup

Install dependencies:
```bash
pip install requests datasets pandas numpy tqdm scikit-learn matplotlib jupyter
```

Then open the config cell in the notebook and add your API key:
```python
API_KEY = "your-api-key-here"
```

No GPU is needed — all inference runs remotely via the API.

---

## Running

Run all cells top to bottom. The notebook will:

1. Load and filter LongBench-v2
2. Sample up to 20 questions per domain
3. For each question, pad the context with same-domain distractors to hit each target length (2K → 64K tokens)
4. Send each padded prompt to the model and record the prediction
5. Save results to `longbench_v2_context_sweep_vapi_qwen.csv`

---

## Results

After the sweep, the analysis cells produce:

- **Accuracy by context length** — how correctness changes as input grows
- **Accuracy by domain** — which task types hold up best under long contexts
- **Trend slopes** — linear regression of accuracy on log₂(tokens) to quantify degradation
- **Latency chart** — average API response time at each context length
- **Error table** — incorrect predictions for manual inspection
