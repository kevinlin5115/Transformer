# Transformer from Scratch for Machine Translation

[![Python](https://img.shields.io/badge/python-v3.7%2B-blue)](https://www.python.org/downloads/)
[![PyTorch](https://img.shields.io/badge/pytorch-v1.10%2B-red)](https://pytorch.org/)

This repository contains a custom implementation of a Transformer model designed for machine translation tasks. The project walks through the core components of the Transformer architecture and demonstrates their application in translating text from one language to another.

## Table of Contents

- [Overview](#overview)
- [Features](#features)
- [Getting Started](#getting-started)
  - [Prerequisites](#prerequisites)
  - [Installation](#installation)
- [Model Architecture](#model-architecture)
- [Acknowledgments](#acknowledgments)

## Overview

The Transformer model, as introduced by Vaswani et al. in "Attention is All You Need", has revolutionized natural language processing (NLP). This project provides an end-to-end implementation of a Transformer for machine translation, demonstrating how to build and train the model from scratch using PyTorch.

## Features

- **Self-Attention Mechanism**: Captures relationships between all words in a sequence, regardless of their position.
- **Multi-Head Attention**: Enhances the model’s ability to focus on different parts of the input.
- **Positional Encoding**: Adds information about the position of each word in the sequence.
- **Layer Normalization and Residual Connections**: Ensures stable and efficient training.

## Getting Started

### Prerequisites

Before you begin, ensure you have the following installed:

- Python 3.7 or higher
- PyTorch 1.10 or higher
- NumPy
- Matplotlib

### Installation

Clone the repository and install the necessary dependencies:

```bash
git clone https://github.com/kevinlin5115/Transformer.git
cd Transformer
pip install -r requirements.txt
```

## Model Architecture

The Transformer architecture is composed of several core components that work together to process input sequences and generate outputs. Below is a detailed explanation of each component:

### Positional Encoding

Transformers do not inherently understand the order of words in a sequence. To give the model a sense of word order, we add positional encodings to the input embeddings. These encodings use sine and cosine functions of different frequencies:

$$ PE_{(pos, 2i)} = \sin\left(\frac{pos}{10000^{2i/d_{\text{model}}}}\right) $$

$$ PE_{(pos, 2i+1)} = \cos\left(\frac{pos}{10000^{2i/d_{\text{model}}}}\right) $$

Where $pos$ is the position in the sequence, $i$ is the dimension, and $d_{\text{model}}$ is the model’s dimensionality. These functions allow the model to distinguish the position of words by providing a unique encoding for each position.

### Self-Attention

Self-attention enables the model to focus on different parts of the input sequence when processing a particular word. It computes a weighted sum of the input sequence, where the weights are determined dynamically.

#### Query, Key, and Value

Each word in the sequence is transformed into three vectors: Query (Q), Key (K), and Value (V). These vectors are generated by multiplying the word's embedding with learned weight matrices:

- **Query (Q)**: Determines which part of the input sequence to focus on.
- **Key (K)**: Represents the words in the sequence to be compared against the query.
- **Value (V)**: Contains the actual information about the word.

The attention score is calculated as:

$$ \text{Attention}(Q, K, V) = \text{softmax}\left(\frac{Q \cdot K^T}{\sqrt{d_k}}\right) V $$

Where $d_k$ represents the dimensionality of the key vectors. This attention score evaluates the importance of each word in the context of the others, guiding the model on where to focus. Consistent with the original Transformer paper, in this project, the model dimension $d_{\text{model}}$ is set to 512, with 8 attention heads, each having a dimension of $d_k = \frac{d_{\text{model}}}{\text{heads}} = \frac{512}{8} = 64$.


### Multi-Head Attention

Instead of performing a single attention operation, the Transformer uses multiple attention heads. Each head processes the input sequence independently and captures different aspects of the relationships between words. The results from all heads are then concatenated and projected to the desired dimension.

This can be represented as:

$$ \text{MultiHead}(Q, K, V) = \text{Concat}(\text{head}_1, \text{head}_2, ..., \text{head}_h)W^O $$

Where $h$ is the number of heads, and $W^O$ is the output projection matrix.

### Feed-Forward Networks

Each position in the sequence is processed by a feed-forward neural network independently. This network consists of two linear transformations with a ReLU activation in between:

$$ \text{FFN}(x) = \max(0, xW_1 + b_1)W_2 + b_2 $$

This network is applied to each position in the sequence, allowing the model to capture more complex relationships.

### Layer Normalization and Residual Connections

To stabilize training and enable deep networks, the Transformer uses layer normalization and residual connections:

- **Layer Normalization**: Normalizes the input across the features dimension, which helps in maintaining a stable distribution of activations.
- **Residual Connections**: Adds the input of a layer to its output before applying the next operation. This helps in preserving the information and gradients during backpropagation.

### Encoder-Decoder Structure

The Transformer model uses an encoder-decoder structure, which consists of several layers of the above components:

- **Encoder**: Each layer in the encoder consists of a multi-head attention mechanism followed by a feed-forward network. The encoder processes the input sequence and generates context vectors.

- **Decoder**: Each layer in the decoder also has a multi-head attention mechanism, but it includes an additional attention layer to focus on the encoder's output. This allows the decoder to generate the output sequence based on both the previous outputs and the encoded context from the input sequence.

### Implementation Details

Key implementation files:

- `model.py`: Contains the definitions of the Transformer’s components including positional encoding, self-attention, multi-head attention, and the full encoder-decoder architecture.
- `dataset.py`: Manages data loading and preprocessing, preparing input sequences for training and evaluation.
- `train.py`: Implements the training loop, including loss calculation, backpropagation, and model evaluation.
- `config.py`: Defines configuration settings and hyperparameters used in training and evaluating the model.

## Acknowledgments

I would like to thank the creators of the following resources for their invaluable contributions and insights, which greatly influenced the development of this project:

- The original Transformer paper: ["Attention is All You Need"](https://arxiv.org/abs/1706.03762) by Vaswani et al., which introduced the groundbreaking architecture.
- The PyTorch community for providing extensive documentation and tutorials.
- [Umar Jamil](https://www.youtube.com/@umarjamilai) for the comprehensive tutorial on building a Transformer model, which served as a guiding resource in this project: [YouTube Video](https://www.youtube.com/watch?v=ISNdQcPhsts&t=6285s).

These resources provided essential knowledge and practical insights that were instrumental in creating this project.

