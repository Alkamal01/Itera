# Itera: Self-improving Multi-agent Systems via Bootstrapped Reasoning

[![License](https://img.shields.io/badge/License-MIT-blue.svg)][#license]
[![Arxiv](https://img.shields.io/badge/arXiv-2502.04780-B31B1B.svg)][#arxiv-paper]

[#license]: https://opensource.org/licenses/MIT
[#arxiv-paper]: https://arxiv.org/abs/2502.04780

## Overview

**Itera** (formerly SiriuS) is a self-improving multi-agent framework designed to continuously enhance reasoning capabilities. It achieves this by maintaining an experience library of successful trajectories and bootstrapping from failed ones, effectively allowing agents to learn from their own interactions.

This repository implements the framework described in the paper [**SiriuS: Self-improving Multi-agent Systems via Bootstrapped Reasoning**](https://arxiv.org/abs/2502.04780).

## Modules

The project is organized into three main multi-agent settings:

*   **`problem_solving/`**: Pipelines for collaborative Question Answering (QA), covering College Physics/Chemistry and PubMedQA-style reasoning.
*   **`actor_critic/`**: An iterative refinement pipeline involving an Actor, a Judgment agent, and a Critic agent.
*   **`competitive/`**: Environments for negotiation and game-theoretic interactions (e.g., Resource Exchange, Ultimatum Game).

## Installation

### Prerequisites

*   Python 3.10+
*   Conda (recommended)

### Setup

1.  **Clone the repository:**
    ```bash
    git clone https://github.com/Alkamal01/itera.git
    cd itera
    ```

2.  **Create and activate the environment:**
    ```bash
    conda env create -f environment.yml
    conda activate itera
    ```

    *Alternatively, you can install dependencies using pip:*
    ```bash
    pip install .
    ```

3.  **Configure API Access:**
    Set your OpenAI API key as an environment variable:
    ```bash
    export OPENAI_API_KEY="your-api-key-here"
    ```

## Usage

### 1. Collect Trajectories

Run the multi-agent system to collect interaction trajectories. For example, to run the physics problem-solving task:

```bash
python problem_solving/PhyChem/get_a_sol.py --model='gpt-3.5-turbo' --task='MMLU_physics' --prompt_type='multi_agent' --mode='generate' --subject='phy'
```

### 2. Process and Regrow

Filter successful trajectories and generate feedback for failed ones:

```bash
# Filter trajectories
python libs/merge.py

# Generate feedback for incorrect solutions
python problem_solving/PhyChem/get_b_feedback.py ...

# Regenerate solutions based on feedback
python problem_solving/PhyChem/get_c_regenerate.py ...
```

### 3. Fine-Tune

Fine-tune your agents using the collected experience library:

```bash
python problem_solving/PhyChem/get_finetune_data.py
python problem_solving/PhyChem/fine_tune.py
```

## Structure

*   `problem_solving/`: Code for reasoning tasks.
*   `actor_critic/`: Code for actor-critic feedback loops.
*   `competitive/`: Code for game-theory scenarios.
*   `libs/`: Shared utility libraries.
*   `dataset/`: Directory for storing training and evaluation datasets.

