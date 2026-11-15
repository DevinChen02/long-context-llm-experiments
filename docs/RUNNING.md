# Setup and run instructions

Use Python 3.10 or later. Create a separate virtual environment for each experiment and run from the experiment's directory: notebooks and scripts use relative file paths. API inference needs credentials and may incur provider charges. No API inference was rerun during this consolidation.

## Context length sweep

From the repository root:

```bash
cd experiments/context_length
python3 -m venv .venv
source .venv/bin/activate
python -m pip install -r requirements.txt
jupyter notebook context_length_sweep.ipynb
```

In the API configuration cell, set `BASE_URL`, `MODEL_NAME`, and `API_KEY`. The imported notebook uses an OpenAI-compatible chat-completions endpoint. Keep credentials local and clear them before saving or sharing a notebook.

Run the cells in order. The notebook downloads LongBench v2, constructs contexts for approximate token budgets `[2000, 4000, 8000, 16000, 32000, 64000]`, calls the API, and exports `longbench_v2_context_sweep_vapi_qwen.csv`. Saved analysis output and plots are already embedded in the notebook; the exported CSV was not included upstream.

## Needle position perturbation

From the repository root:

```bash
cd experiments/needle_position
python3 -m venv .venv
source .venv/bin/activate
python -m pip install -r requirements.txt
jupyter notebook
```

1. Run `544_longbench_quality_generation.ipynb` to download LongBench v2 and QuALITY and generate `longbench_v2_quality_needle.json`. The full generated dataset is not included in the source repository; its README also links to a downloadable copy.
2. Open `544_longbench_quality_retrieve_eval.ipynb`. Set `VAPI_BASE_URL`, `VAPI_API_KEY`, and `MODEL` in the configuration cell. Check the model context limit and input dataset path.
3. Run evaluation and export `model_stats_<model>.json`. Repeat for each model, then run the comparison cells to produce figures.

The imported `model_stats_*.json` files and `figures/` contain existing results. The final latency comparison uses manually entered measurements in `LATENCY_DATA`; update those values when evaluating new models.

## Prompting variants

The upstream `requirements.txt` is retained unchanged and lists local-inference packages. The added `requirements-api.txt` covers the imports used by the API scripts, including both provider SDKs.

From the repository root:

```bash
cd experiments/prompting_variants
python3 -m venv .venv
source .venv/bin/activate
python -m pip install -r requirements-api.txt
export OPENAI_API_KEY="your-key"

python pred.py -p openai -m gpt-5-mini --prompt_variant direct
python pred.py -p openai -m gpt-5-mini --prompt_variant extract_then_answer
python result.py
```

Models must appear in `config/model2maxlen.yaml`. API-key environment-variable names are configured in `config/api_key_name.yaml`; `--api_key_name` overrides them. `--base_url` supports a custom OpenAI-compatible endpoint, provided it supports the Responses API used by this implementation. For available options, run `python pred.py --help` after installing dependencies.

Predictions are written to `results/<model>_<variant>.jsonl`. Existing question IDs are skipped on subsequent runs. Choose a fresh `--save_dir` when you want new predictions rather than resuming an existing run. `result.py` reads the default `results/` folder and writes `result.tsv`, including accuracy and instruction-format failure rates by difficulty and context length.

To evaluate the bundled predictions without installing dependencies or making API calls:

```bash
cd experiments/prompting_variants
python3 result.py
```

### Imported implementation notes

The scripts and notebooks retain their original behavior. These details matter when reproducing results:

- In the context sweep, context size is estimated as characters divided by four. Original contexts are truncated when they exceed the target budget, so evidence availability can change along with length.
- The sweep constructs a domain-balanced sample, but its inference loop indexes `work_df` with the reset indices of `eval_df`. It therefore evaluates different rows from the intended sample. Account for this before using the notebook for a new controlled study.
- The prompting code's Gemini branch does not forward the API key into `GeminiProvider`; that branch needs a code correction before fresh inference. Saved Gemini predictions can still be evaluated offline.
- Both API provider SDKs are imported by the prompting package, even when only one provider is selected.

The added dependency lists are unpinned convenience lists, not tested environment lockfiles. Verification of this import covers source integrity, syntax, notebook structure, documentation links, and offline scoring; live API compatibility was not tested.
