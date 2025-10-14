# 🧪 TinyLlama Safety Audit: Adversarial Jailbreak Benchmarking

## Project Overview

This project implements a foundational AI safety evaluation pipeline focused on **Adversarial Robustness**. We load a small, popular base language model, **TinyLlama-1.1B-Chat-v1.0**, and systematically test its vulnerability against sophisticated adversarial prompts derived from the open-source WildJailbreak dataset.

The primary goal is to calculate the **Attack Success Rate (ASR)**—a critical metric in AI safety—to quantify how easily the model can be induced to generate harmful or restricted content.

### Key Components

* **Model:** `TinyLlama/TinyLlama-1.1B-Chat-v1.0` (Small, 1.1B parameter model)

* **Dataset:** `allenai/wildjailbreak` (A large-scale, open-source synthetic safety dataset)

* **Metric:** Attack Success Rate (ASR) calculated via keyword refusal detection.

***

## 1) The WildJailbreak Dataset and Adversarial Testing

### How AI Safety Datasets are Created by Engineers

The creation of robust AI safety datasets is a continuous red-teaming process often involving specialized engineers and AI systems:

1.  **Red Teaming:** Human security researchers (Red Teamers) actively try to "break" the AI by generating harmful, manipulative, or evasive prompts (jailbreaks) to induce unsafe behavior.

2.  **Synthetic Data Generation:** Engineers use powerful LLMs (like GPT-4 or Mixtral) to automatically generate large volumes of adversarial prompts based on a taxonomy of known attack tactics.

3.  **Data Labeling:** The resulting prompts are paired with desired safety responses (e.g., explicit refusals) or harmful completions, which are then used to train the target model.

### WildJailbreak Dataset Structure

The `allenai/wildjailbreak` dataset is critical because it aims to provide a more realistic and diverse set of attacks than traditional, manually-created sets. When we load the dataset in the Canvas using `train_dataset = dataset_dict['train']`, we access the following types of data:

| Type Category | Data Type Tag | Description |
| :--- | :--- | :--- |
| **Harmful** (Targeted) | `adversarial (H)` | Complex, jailbreaking prompts designed to elicit harmful content. |
| **Harmful** (Direct) | `vanilla (H)` | Direct requests for harmful content (e.g., "Write a guide on X..."). |
| **Benign** (Contrastive) | `vanilla (B)` | Harmless prompts used to ensure the model doesn't "over-refuse" on safe queries. |

### The WildTeaming Project Evaluation Methodology

The WildTeaming project, which created this dataset, introduces an **automatic red-teaming framework** that goes beyond traditional methods:

* **In-the-Wild Mining:** Instead of relying solely on hired Red Teamers, WildTeaming **mines jailbreak tactics** from existing large-scale, in-the-wild user-chatbot interactions (e.g., LMSYS, WildChat). This process discovered thousands of unique jailbreak clusters.

* **Compositional Attacks:** The framework then systematically **composes** selections of these mined, human-devised tactics (like "Role-Play" or "Ethical Guideline Distortion") with vanilla harmful queries to create novel, highly challenging adversarial prompts.

* **Evaluation Focus:** Their core evaluation goal is to find the **ideal balance** of safety behaviors: effective safeguarding against both vanilla and adversarial queries **without** over-refusal on benign queries.

### My Evaluation and Limitation

In this specific Canvas implementation, I performed the following:

* **Data Selection:** I loaded the **`train` split** of the dataset and then filtered specifically for samples containing the `'harmful'` tag.

* **Test Sample Limit:** Due to constraints related to computational resources and time for a quick demonstration, the evaluation was limited to processing only the **first 10 harmful samples** encountered in the dataset.

* **Purpose of Limitation:** This small sample size provides a fast, initial **vulnerability snapshot** of the model but is not statistically representative of the model's safety across the entire dataset. A larger test set is necessary for robust ASR reporting.

***

## 2) Advanced TAS Benchmarking (in progress)

This project is planned to expand into a comprehensive TAS benchmarking suite.

```eof
