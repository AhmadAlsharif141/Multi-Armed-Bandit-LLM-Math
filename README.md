# Multi-Armed Bandit LLMs for Solving Mathematics

Research paper presented at **Smart Tech Conference (2025)**.

**Authors:** Ahmad Alsharif, Omar Alzoubi, Yazeed Alsaedi, Soumaya Chaffar
Department of Artificial Intelligence, University of Prince Mugrin, Saudi Arabia

> 📄 Not yet formally published — presented at Smart Tech Conference 2025. Code available upon request from the corresponding author.

## Abstract

We propose a dual-LLM framework for mathematical problem solving, pairing an open-source solver (LLaMA) with an external validator (GPT-4). To address the limitations of fixed prompting strategies, we introduce a multi-armed bandit agent that dynamically selects among four prompt styles — decomposition, analogy, formal proof, and backward solving — based on the domain of the problem. The system adapts over time to identify which strategy is most effective for domains such as algebra, geometry, and calculus.

## Key Idea

Most LLM math-solving pipelines use a single, fixed prompting strategy for every problem. This project treats each prompting strategy as an "arm" in a multi-armed bandit, learning — through an epsilon-greedy policy — which strategy works best for which math domain (algebra, geometry, calculus, probability, number theory), rather than relying on one-size-fits-all prompting.

## System Architecture

1. **Problem Classification** — GPT-4 infers the math domain of the input problem
2. **Multi-Armed Bandit Agent** — selects a prompt strategy (decomposition, analogy, formal proof, or backward solving) based on historical success rates for that domain, using epsilon-greedy exploration/exploitation
3. **LLM Solver** — LLaMA 3.2-3B-Instruct generates a solution using the selected prompt
4. **LLM Validator** — GPT-4 checks the solution for correctness and logical coherence, returning a correctness flag and partial score
5. **Performance Tracker** — updates success/failure records per strategy–domain pair, feeding back into the bandit's future decisions

## Dataset

Evaluated on a stratified sample of 50 problems from **GSM8K**, supplemented with GPT-4-generated synthetic problems to balance domain coverage (algebra, geometry, probability, number theory, calculus).

## Results

| Agent | Accuracy | Mean Score | Latency (s) |
|---|---|---|---|
| Baseline (fixed CoT) | 36% (18/50) | 0.806 | 278 ± 16 |
| **Bandit-enhanced (ours)** | **44% (22/50)** | **0.830** | **147 ± 12** |

- **+8 percentage points** accuracy improvement over the fixed chain-of-thought baseline
- **~47% reduction** in average latency (278s → 147s), driven by more concise, domain-tailored prompts
- Largest gains in **algebra (+20pp)** and **geometry (+30pp)**
- Decomposition emerged as the most consistently successful strategy, particularly effective for smaller models like LLaMA-3B

## Limitations

- Dependence on GPT-4 as a closed-source validator (reproducibility concern)
- Small evaluation set (50 problems) due to API/compute constraints
- No symbolic verification for algebraic reasoning
- Probability domain performance declined, indicating current prompt templates don't suit combinatorial/case-based reasoning

## Future Work

- Full evaluation on the complete GSM8K dataset
- Replace GPT-4 validator with a lightweight, fine-tuned open-source verifier
- Explore Thompson-sampling bandits to reduce early exploration cost

## Citation

If referencing this work, please cite the authors and Smart Tech Conference (2025) presentation.
