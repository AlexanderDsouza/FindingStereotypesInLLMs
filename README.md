# FindingStereotypesInLLMs

This repository contains the code and experimental results for my Master's Thesis in Computer Science at the University of California, Davis. The project investigates the internal mechanisms of GPT-2 and Llama 3.2 to determine if societal stereotypes are localized in specific "bias neurons" or distributed across the model's residual stream.

## 🚀 Key Insights
* **The Pathway Hypothesis:** Stereotypes are not "bugs" isolated in specific neurons; they are distributed directions in the residual stream. 
* **Early Encoding:** Word Token Embeddings (WTE) alone can classify stereotypical vs. anti-stereotypical sentences with **73.4% accuracy** before a single attention layer is applied.
* **High-Ratio Outliers:** We identified individual neurons with activation ratios as high as $7 \times 10^5$ for stereotypical prompts, yet single-neuron ablation yields marginal causal shifts ($<0.15\%$), proving significant model redundancy.

---

## 📂 Repository Contents

The experimental work is split into two primary methodologies:

### 1. `CXAD_First_Experiment.ipynb`
**Goal:** Locate "Contrastive Neurons" using a framework inspired by Contrastive Explanations for Anomaly Detection (CXAD).
* Extracts activations from WTE, Multi-Head Attention (MHA), and Feed-Forward Network (FFN) layers.
* Calculates relative activation ratios to identify neurons that "over-activate" for stereotypes.
* Explores the "WTE vs. Positional" hypothesis to determine if bias is embedded at the entry point of the model.

### 2. `Attention_Heads_Second_Experiment_GPT2.ipynb`
**Goal:** Map "Bias Fingerprints" specifically within the GPT-2 Small architecture.
* Trains a supervised Multi-Layer Perceptron (MLP) probe on frozen internal activations.
* Utilizes **Monte Carlo Shapley values** to identify the top 10% of attention heads driving biased classifications.
* Performs causal intervention (ablation) and evaluates performance using StereoSet metrics.

### 3. `Attention_Heads_Second_Experiment_Llama.ipynb`
**Goal:** Scalability and verification on modern architectures (Llama 3.2 1B).
* Replicates the probing and Shapley value methodology on a larger, more modern decoder-only model.
* Compares the localization of bias between GPT-2 and Llama 3.2.
* Demonstrates that while Llama is more sophisticated, it shares similar "bias pathways" in the residual stream.

---

## 📊 Summary of Results



Ablating the identified neurons resulted in a reduction in social bias (shifting the Stereotype Score toward the 50% ideal) while maintaining or improving the overall iCAT score.

| Model | Configuration | SS (Ideal 50) | LMS (Ideal 100) | iCAT (Ideal 100) |
| :--- | :--- | :--- | :--- | :--- |
| **GPT-2 Small** | Baseline | 55.97 | 90.78 | 79.28 |
| **GPT-2 Small** | **Ablated** | **55.85** | 90.66 | **79.39** |
| **Llama 3.2 (1B)** | Baseline | 58.22 | 91.14 | 75.33 |
| **Llama 3.2 (1B)** | **Ablated** | **57.68** | 90.97 | **76.14** |
| **Ideal Model** | --- | **50.00** | **100.0** | **100.0** |

---

## 🛠️ Installation & Requirements

To set up the environment, clone the repository and install the dependencies:

```bash
pip install -r requirements.txt
---

## 🎓 Citation
If you use this code or these findings in your research, please cite:
**Alexander D'Souza. "Can We Locate and Prevent Stereotypes in LLMs?" Master's Thesis, UC Davis, 2026.**
