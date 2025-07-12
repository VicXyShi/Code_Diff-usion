# Process and Results

This document outlines the methodology and findings of our project:
**Code Diff-usion: Generative AI for Smarter Code Reviews**

It complements the main [README](../README.md) by providing additional detail on our experimental process, challenges, and insights gained.

---

## Approach

We set out to automate post-review code refinement, generating revised Java methods that reflect typical bug fixes, cleanups, and stylistic improvements.

We evaluated two generative modeling approaches:

* **Transformer-based baseline (CodeReviewer)** — an encoder-decoder transformer architecture, re-implemented and trained following Tufano et al. (2019).
* **Diffusion-based model (Diffusion-LM)** — a supervised fine-tuned diffusion language model (`LLaDA-8B-Instruct`) adapted for discrete code-to-code generation.

Our focus was on isolated Java methods, reflecting the granularity of the Tufano dataset.

---

## Dataset

We used the dataset introduced by Tufano et al. (ICSE 2019), containing approximately 92,000 paired examples of Java methods **before** and **after** human revision.

* Input: raw method prior to review.
* Target: revised method post-review.
* Train/validation/test splits prepared accordingly.

---

## Methodology

### Baseline: Transformer

* Implemented as an encoder-decoder model trained from scratch on our dataset.
* Served as a performance reference point for this task.

### Diffusion Model: LLaDA

* Loaded `LLaDA-8B-Instruct` base model from Hugging Face.
* Applied LoRA (Low-Rank Adaptation) to efficiently fine-tune for our task.
* Fine-tuning performed with the `trl` Supervised Fine-Tuning Trainer on 1,000 training and 200 validation examples (due to compute constraints).
* Training included early stopping and monitoring of validation loss.

We implemented a custom `LossComputingTrainer` to correctly compute loss over relevant tokens.

---

## Evaluation

We evaluated on a held-out set of 100 examples.
Metrics:

* **BLEU** and **CodeBLEU**: n-gram and structural similarity.
* **Edit Distance**: token-level difference between generated and reference code.

We also conducted qualitative human inspection of generated code to assess readability and alignment with typical review goals.

---

## Key Results

We evaluated five models on a held-out test set of 100 Java method pairs.
Metrics reported:

* **CodeBLEU** — measures syntactic and semantic correctness of generated code.
* **Corpus BLEU** — standard BLEU metric for sequence similarity.
* **Edit Distance** — number of token-level edits required to match the reference. (lower is better)

| Model                              | CodeBLEU  | Corpus BLEU | Edit Distance |
| ---------------------------------- | --------- | ----------- | ------------- |
| **CodeReviewer**                   | 74.61     | 78.76       | 49.96         |
| **CodeGPT**                        | **78.68** | **78.76**   | **46.53**     |
| **Gemini-2.0 Flash (LLM)**         | 63.56     | 57.78       | 106.20        |
| **LLaDA-8B-Instruct (Vanilla)**    | 64.33     | 59.25       | 94.50         |
| **LLaDA-8B-Instruct (Fine-Tuned)** | 65.32     | 61.68       | 90.42         |

### Observations:

* The **CodeGPT** baseline achieved the best overall performance across all metrics.
* Our fine-tuned diffusion model (**LLaDA-8B-Instruct Fine-Tuned**) improved upon the vanilla model by a small but noticeable margin (+1 point CodeBLEU, lower edit distance).
* The diffusion-based model still lags behind transformer baselines, suggesting room for further architectural adaptation or training scale.

---

## Future Work

* Explore larger-scale fine-tuning to unlock the full potential of diffusion models.
* Investigate hybrid approaches combining Transformer and Diffusion techniques.
* Evaluate on additional programming languages and multi-method contexts.

---
*For questions or more details, please see our [full report](project_report.pdf) or contact the authors.*

