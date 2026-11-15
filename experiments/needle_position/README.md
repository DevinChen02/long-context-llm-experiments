# Longbenchv2-needle position perturbation experiment

## The experiment is run on MacOS (M-series chips)

## Step 1: Dataset Generation
Generate the QuALITY-Needle dataset by injecting QuALITY articles (as needles) into LongBench v2 samples. This is done in the notebook `544_longbench_quality_generation.ipynb`. All instructions are included inside the notebook. The generated dataset is saved as `longbench_v2_quality_needle.json`.
For simplicity, `longbench_v2_quality_needle.json` zip can be downloaded here https://drive.google.com/file/d/1lhIO-o7TRXS-rfTyjKVBaxWVeB-VVSWY/view?usp=sharing. You can unzip it and directly use it for Step 2.

## Step 2: Running the Needle position perturbation experiment
Run the experiment on the `longbench_v2_quality_needle.json` dataset using the notebook `544_longbench_quality_retrieve_eval.ipynb`. All instructions are included inside the notebook. The notebook will do the following:
- Run the experiment for each model. 
- Save each model's experiment result in `model_stats_<model_name>.json`. (5 models used in this project: claude-opus-4-7, deepseek-v3, gemini-3.1-flash-lite-preview, gpt-5.4-nano, qwen3-vl-flash-lite-preview). 
- Generate the result plots, and saved in the `figures` folder. The result plots include:
  - retrieval failure rate by needle position for all models
  - retrieval failure rate by context length for all models
  - retrieval failure rate by context domain for all models
  - mean inference latency by context length for all models
