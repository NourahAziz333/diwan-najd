# Diwan Najd: Najdi Poetry Language Model

NoteBook: https://colab.research.google.com/drive/1886QjzvgA4Av-6B0vqpMAXhznCI3rM4-?usp=sharing
## Overview

Diwan Najd is a **small Arabic language model** trained to understand and generate responses related to Arabic poetry using the Najdi dialect. The model is trained from scratch using a Transformer-based architecture implemented in PyTorch.

The objective of the project is to demonstrate how a lightweight language model can be trained to interpret classical Arabic poetry and produce dialect-based explanations and responses.


The training pipeline consists of **two stages**:

1. **Pretraining** on Arabic text and poetry corpora

2. **Supervised Fine-Tuning (SFT)** using instruction-based prompts

The final model contains approximately **41.6 million** parameters.

#
# Dataset Sources
## Modern Standard Arabic (MSA)

The general Arabic corpus was collected using the HuggingFace Datasets library.

**Source:**

[Omartificial-Intelligence-Space/FineWeb2-MSA](https://huggingface.co/datasets/Omartificial-Intelligence-Space/FineWeb2-MSA)

These datasets provide grammatical structure and vocabulary coverage for Modern Standard Arabic.
#
## Arabic Poetry Dataset

The poetry dataset contains classical Arabic poetry used to teach the model poetic style and vocabulary.

**Source:**

[HuggingFace Arabic Poetry datasets](https://huggingface.co/datasets/qafiyah/classical-arabic-poetry)

The dataset **includes**:

**Classical Arabic poetry**

**Pre-Islamic poetry**

**Literary Arabic verses**

These texts allow the model to learn **poetic structure** and **figurative language**.

#
## Data Preprocessing

**Several preprocessing steps were applied to ensure data quality:**

- removal of extremely short samples

- filtering invalid text entries

- normalization of Arabic characters

- removal of noisy text

**The dataset was split into:**

- training set

- validation set

Validation ratio:

VAL_RATIO = 0.1

# 


## Tokenization

Tokenization was performed using the HuggingFace SaudiBERT Tokenizer https://huggingface.co/faisalq/SaudiBERT.


# 

## Model Architecture

The model is a GPT-style Transformer architecture implemented from scratch in PyTorch.

**Configuration:**

**Embedding Dimension: 256**
**Attention Heads: 8**
**Transformer Layers: 4**
**Context Length: 128**
**Dropout Rate: 0.1**

**Total parameters:**

**41,590,784 parameters**

**Core components implemented:**

- Multi-Head Self-Attention

- Feed-Forward Networks

- Residual Connections

- Layer Normalization

- Token Embeddings

- Positional Embeddings



# 

# Phase 1 — Pretraining

During pretraining the model learns **general Arabic language patterns**.

**Training objective:**

- Next Token Prediction

- The model predicts the next token in a sequence.

- Loss function:

- Cross Entropy Loss

**Training configuration:**

Batch Size: 8

Learning Rate: 3e-4

Epochs: 5

Context Length: 128

Stride: 64

#

# Phase 2 — Supervised Fine-Tuning (SFT)

After pretraining the model was fine-tuned using an **instruction-following dataset**.

**Each training sample follows this structure:**

**instruction:**
**input:**
**output:**

**Example:**

**instruction:**
اشرح معنى البيت التالي باللهجة النجدية

**input:**
إذا غامرت في شرف مروم

**output:**
يقصد ان اللي يبي المعالي ما يرضى بالقليل ولازم يغامر ويتعب عشان يوصل

**During training:**

- prompt tokens are masked

- loss is calculated only on response tokens

This allows the model to learn how to generate answers instead of copying prompts.

#

## Fine-Tuning Loss Curve

The model shows **consistent improvement** during **fine-tuning**.

| Epoch | Train Loss | Validation Loss |
| ----- | ---------- | --------------- |
| 1     | 5.00       | 5.11            |
| 2     | 2.57       | 3.18            |
| 3     | 1.84       | 2.17            |
| 4     | 1.02       | 1.70            |
| 5     | 0.55       | 1.30            |
| 6     | 0.27       | 1.06            |
| 7     | 0.21       | 0.86            |
| 8     | 0.12       | 0.71            |


#

Both training and validation losses **decrease steadily**, indicating that the model successfully learns instruction-following patterns during fine-tuning.

**Fine-tuning Loss plot:**

<img width="728" height="411" alt="image" src="https://github.com/user-attachments/assets/67f86ac4-e39e-433e-9626-0e44698465de" />

#

## Evaluation

**Two evaluation approaches were used.**

## 1. Perplexity

Perplexity measures how well the model predicts the next token.

**Formula:**

**Perplexity = exp(loss)**

**Lower** perplexity indicates **better** language modeling performance.

#
## 2. LLM-as-Judge Evaluation

Generated outputs were evaluated using a larger language model acting as an automatic judge.

**Evaluation criteria:**

- correctness

- clarity

- Najdi dialect quality

- instruction following

- fluency

**Example evaluation tasks:**

- poetry explanation

- poetry generation

- Najdi dialect question answering


  ## Evaluation Results

  ## Average Overall Score

**Overall Model Quality ≈ 2 / 5**

#

## Identified Failure Modes

The evaluation revealed several common issues.

- **Repetition**: The model frequently repeats phrases during generation.

- **Semantic Drift**: Responses sometimes begin correctly but gradually lose context.

- **Weak Poetry Interpretation**: The model sometimes generates generic poetic phrases rather than explaining the verse meaning.

- **Inconsistent Dialect Usage**: Najdi dialect appears in some responses but is not consistently applied.

#

## Limitations

The limitations observed are expected due to:

**- small model size (~41M parameters)**

**- limited fine-tuning dataset**

**- relatively short training duration**

#

# Conclusion

Diwan Najd demonstrates that a **small transformer-based language model** can be trained to generate Arabic text and partially follow instruction-based prompts related to poetry and dialect.

Although the model shows **limitations** in semantic understanding and coherence, the training pipeline successfully enables the model to produce stylistically Arabic outputs and respond to structured prompts.
