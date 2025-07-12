# Code Diff-usion: Generative AI for Smarter Code Reviews

## Overview

This project explores the use of generative models to automate **post-review code refinement** — generating improved versions of code that reflect typical bug fixes, cleanups, and stylistic changes usually performed during code review.

We investigate and compare two generative approaches:

* **Transformer-based model (CodeReviewer)**: an encoder-decoder transformer model pre-trained for code refinement.
* **Diffusion-based model (Diffusion-LM)**: a discrete diffusion model fine-tuned for code-to-code rewriting.

By benchmarking these models on a large dataset of real-world Java method revisions, we aim to assess the potential of diffusion models in the domain of code review automation.

---

## Task Formulation

**Input**: a Java method before code review.
**Output**: the same method, revised to reflect typical post-review edits.

We focus on the Java language and isolated methods (not full files or projects).

---

## Dataset

We use the dataset by Tufano et al. (ICSE 2019), which contains \~92,000 pairs of Java methods (pre- and post-review versions).

* Format: tokenized `.txt` files where each line contains a pair of methods separated by a delimiter.
* Each pair represents a real-world edit made in a GitHub commit.

---

## Methods

### Baseline: Transformer (CodeReviewer)

* Encoder-decoder transformer model trained on the Tufano dataset.
* Pre-trained for code review tasks including comment generation and refinement.

### Diffusion Model: Diffusion-LM

* Fine-tuned a Hugging Face-compatible diffusion model (`LLaDA-8B-Instruct`) using supervised fine-tuning (SFT) on the Tufano dataset.
* The model is adapted for structured, discrete code generation.

---

## Challenges

* Code semantics are subtle and context-dependent.
* The model sees only isolated methods, limiting its ability to reason about broader codebase context.
* Fine-tuning a text-to-text model for structured Java code introduces syntactic constraints.

---

## Evaluation

* **Quantitative**:

  * BLEU
  * CodeBLEU
  * Edit distance
* **Qualitative**:

  * Human inspection of generated outputs for meaningfulness and alignment with review intent.

The transformer baseline is used as a point of comparison for the diffusion model.

---

## Alternative Approaches Considered

* **LoRA-based fine-tuning**: more efficient but may sacrifice performance.
* **Masked language models (e.g., CodeBERT)**: more suited for classification or token prediction, less for full sequence generation.
* Converting to an image/text task for image-based diffusion models (inefficient and less relevant).

---

## References

* Tufano et al. (2019) – *Towards Automating Code Review Activities*. [arXiv:2101.02518](https://arxiv.org/pdf/2101.02518)
* Li et al. (2022) – *Diffusion-LM Improves Controllable Text Generation*. [arXiv:2205.14217](https://arxiv.org/abs/2205.14217)
* Austin et al. (2021) – *Structured Denoising Diffusion Models in Discrete Space*. [arXiv:2107.03006](https://arxiv.org/abs/2107.03006)
* Rasheed et al. (2024) – *AI-powered Code Review with LLMs: Early Results*. [arXiv:2404.18496](https://arxiv.org/pdf/2404.18496)

---

## Authors

* Victoria Shi
* Anna Yang
* Dana Hua


MIT Course 6.C511 Spring 2025
