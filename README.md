# Awesome Code World Models

[![Awesome](https://awesome.re/badge.svg)](https://awesome.re)
![Last updated](https://img.shields.io/badge/last%20updated-September%202026-blue)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](#contributing)

A curated collection of papers, models, datasets, benchmarks, and resources on **Code World Models (CWMs)**—learned models that predict how executable software environments change.

> This is a young and fast-moving area. The list uses a deliberately strict scope so that it does not become another general collection of code LLMs or coding agents.

## What is a Code World Model?

A **Code World Model** is a learned predictive model of an executable software environment. Given a current state $s_t$—such as source files, runtime memory, a repository, or a terminal session—and an action $a_t$—such as executing a statement, editing a file, invoking an API, or running a command—it predicts an execution-relevant consequence:

$$
\hat{p}(s_{t+1}, o_t, r_t, d_t \mid s_t, a_t)
$$

The prediction may represent the next program or environment state, an execution trace, stdout/stderr, an exception, a test result, resource usage, a reward, or termination. A CWM becomes especially useful when these predictions replace or augment real execution in planning, search, generation, verification, or agent training.

### Inclusion Criteria

- Learned transition models for interpreters, debuggers, terminals, repositories, build systems, compilers, and test runners.
- Learned execution, trace, output, error, test, resource, or reward predictors.
- Benchmarks that directly measure software-state or execution-outcome prediction.
- Closely related systems that synthesize executable world models *as code*, listed separately.

### Exclusion Criteria

- Ordinary code completion, generation, repair, or static code understanding.
- Coding agents that only call real tools and do not learn environment dynamics.
- Verifiers trained only on textual preferences, without execution-grounded targets.
- General world models unrelated to executable software environments.

<p align="center">
  <img src="assets/code-world-model-overview.png" width="100%" alt="A Code World Model predicts the next software state, output, error, test result, or reward from the current code-world state and an action, allowing an agent to plan over imagined futures." />
</p>

## Contents

- [Core Models](#core-models)
  - [Repository and Terminal World Models](#repository-and-terminal-world-models)
  - [Program Execution and State Models](#program-execution-and-state-models)
- [Evaluation and Analysis](#evaluation-and-analysis)
- [Benchmarks](#benchmarks)
- [World Models as Code](#world-models-as-code)
- [Contributing](#contributing)

## Tag Legend

- `transition` — predicts action-conditioned next states or observations
- `trace` — predicts intermediate program execution
- `outcome` — predicts outputs, errors, tests, resources, or termination
- `reward` — predicts task success or scalar feedback
- `repo` / `terminal` — models repository-scale or command-line environments
- `open model` — public model weights are available

## Core Models

### Repository and Terminal World Models

- **Qwen-AgentWorld** — *Qwen-AgentWorld: Language World Models for General Agents*. arXiv 2026. A native next-observation model trained across seven agent environments; its Terminal and SWE domains predict shell, file-system, edit, compiler, and test feedback. `transition` `repo` `terminal` `open model`
  [Paper](https://arxiv.org/abs/2606.24597) · [Code](https://github.com/QwenLM/Qwen-AgentWorld) · [Model](https://huggingface.co/Qwen/Qwen-AgentWorld-35B-A3B) · [AgentWorldBench](https://huggingface.co/datasets/Qwen/AgentWorldBench)

- **ECHO** — *ECHO: Terminal Agents Learn World Models for Free*. arXiv 2026. Adds an environment-observation prediction objective to terminal-agent RL, learning command effects from stdout, errors, files, logs, and traces already present in rollouts. `transition` `terminal`
  [Paper](https://arxiv.org/abs/2605.24517)

- **SWE-World** — *SWE-World: Building Software Engineering Agents in Docker-Free Environments*. arXiv 2026. Replaces physical repository execution with learned transition and reward models that simulate command outcomes and final test feedback. `transition` `reward` `repo`
  [Paper](https://arxiv.org/abs/2602.03419) · [Code](https://github.com/RUCAIBox/SWE-World)

- **CWM** — *CWM: An Open-Weights LLM for Research on Code Generation with World Models*. arXiv 2025. A 32B model mid-trained on Python execution traces and agent interactions in containerized software environments. `transition` `trace` `repo` `open model`
  [Paper](https://arxiv.org/abs/2510.02387) · [Project](https://ai.meta.com/research/publications/cwm-an-open-weights-llm-for-research-on-code-generation-with-world-models/) · [Code](https://github.com/facebookresearch/cwm) · [Model](https://huggingface.co/facebook/cwm)

### Program Execution and State Models

- **Neural Debugger** — *Towards a Neural Debugger for Python*. arXiv 2026. Models forward and inverse Python execution conditioned on debugger actions such as step-into, step-over, step-return, and breakpoints. `transition` `trace` `outcome`
  [Paper](https://arxiv.org/abs/2603.09951)

- **Self-Execution Simulation** — *Self-Execution Simulation Improves Coding Models*. arXiv 2026. Trains step-by-step execution simulation with supervised traces and verifiable RL, then uses predicted test outcomes for self-verification and repair. `trace` `outcome`
  [Paper](https://arxiv.org/abs/2604.03253)

- **ExecVerify** — *ExecVerify: White-Box RL with Verifiable Stepwise Rewards for Code Execution Reasoning*. ACL 2026. Rewards correct next-statement, variable-value, variable-type, and final-output predictions before transferring the capability to code generation. `trace` `outcome`
  [Paper](https://aclanthology.org/2026.acl-long.631/) · [Code, Data, and Models](https://github.com/tlx000000001/ExecVerify)

- **Execution Tuning** — *What I Cannot Execute, I Do Not Understand: Training and Evaluating LLMs on Program Execution Traces*. arXiv 2025. Trains line- and instruction-level execution models and studies compact and dynamic state representations for long rollouts. `trace` `outcome`
  [Paper](https://arxiv.org/abs/2503.05703)

- **SemCoder** — *SemCoder: Training Code Language Models with Comprehensive Semantics Reasoning*. NeurIPS 2024. Learns execution-aware semantic traces and execution monologues for generation and repair. `trace` `outcome`
  [Paper](https://proceedings.neurips.cc/paper_files/paper/2024/hash/6efcc7fd8efeee29a050a79c843c90e0-Abstract-Conference.html) · [Code, Data, and Models](https://github.com/ARiSE-Lab/SemCoder)

- **TRACED** — *TRACED: Execution-Aware Pre-training for Source Code*. ICSE 2024. Predicts execution coverage and quantized runtime variable states from source code and inputs. `trace` `outcome`
  [Paper](https://arxiv.org/abs/2306.07487) · [Code](https://github.com/ARiSE-Lab/TRACED_ICSE_24)

- **CodeExecutor** — *Code Execution with Pre-trained Language Models*. Findings of ACL 2023. A learned Python executor that predicts line-by-line traces and final outputs, with execution pre-training transferred to code intelligence tasks. `trace` `outcome`
  [Paper](https://aclanthology.org/2023.findings-acl.308/) · [Code and Data](https://github.com/microsoft/CodeBERT/tree/master/CodeExecutor)

- **Learning to Execute** — *Learning to Execute*. arXiv 2014. A foundational study training recurrent networks to predict program outputs directly from source code and inputs. `outcome`
  [Paper](https://arxiv.org/abs/1410.4615) · [Code](https://github.com/wojciechz/learning_to_execute)

## Evaluation and Analysis

- **Towards Evaluation of Implicit Software World Models in Coding LLMs** — DL4Code at ICML 2026. Evaluates test outcomes, exceptions, memory, runtime, and profiler predictions on library-level cases derived from SWE-bench Verified.
  [Paper](https://arxiv.org/abs/2606.27406) · [Code](https://github.com/JetBrains-Research/cwm-execution-tracer) · [Data](https://huggingface.co/datasets/JetBrains-Research/cwm-benchmarks-dl4c-benchmark)

- **Debugging Code World Models** — 2026. Separates action-generation failures from state-propagation failures in long execution rollouts and analyzes the effect of dense state supervision.
  [Paper](https://arxiv.org/abs/2602.07672) · [Blog](https://babak70.github.io/code-world-models-blog/)

## Benchmarks

Benchmarks are listed here only when they directly test learned execution or software-environment prediction; they are not themselves Code World Models.

- **2026**
  - **AgentWorldBench** — Next-observation prediction across agent environments, including dedicated Terminal and SWE subsets. [Dataset](https://huggingface.co/datasets/Qwen/AgentWorldBench)
  - **DexBench** — Paired forward-execution and counterfactual input-mutation tasks. [Paper](https://aclanthology.org/2026.acl-long.735/)
  - **CoRE** — Fine-grained intermediate-state and implementation-invariance evaluation. [Paper](https://aclanthology.org/2026.findings-acl.460/) · [Code](https://github.com/ZJUSig/CoRE)
  - **CES** — Evaluates the coherence and cross-input consistency of code-execution reasoning. [Paper](https://arxiv.org/abs/2510.15079) · [Code](https://github.com/Intelligent-CAT-Lab/CES)
  - **PLSemanticsBench** — Tests final states, traces, and semantic rules under both standard and deliberately altered programming-language semantics. [Paper](https://arxiv.org/abs/2510.03415) · [Code](https://github.com/EngineeringSoftware/PLSemanticsBench)

- **2025**
  - **SURGE** — 1,160 problems spanning multi-language, repository-level, scientific, environment-dependent, and high-cost surrogate execution. [Paper](https://aclanthology.org/2025.emnlp-main.162/) · [Code and Data](https://github.com/Imbernoulli/SURGE)
  - **R-Eval** — Evaluates coverage, intermediate state and type, next statement, output, and execution consistency. [Paper](https://arxiv.org/abs/2403.16437) · [Project](https://r-eval.github.io/)
  - **ThrowBench** — Runtime-exception prediction over 2,466 programs in four languages. [Paper](https://arxiv.org/abs/2503.04241) · [Code and Data](https://github.com/giganticode/throwbench)
  - **BigO(Bench)** — Runtime and memory-complexity prediction over programming problems and solutions. [Paper](https://arxiv.org/abs/2503.15242) · [Code and Data](https://github.com/facebookresearch/BigOBench)

- **2024**
  - **CRUXEval** — Input and output prediction for short Python functions. [Paper](https://proceedings.mlr.press/v235/gu24c.html) · [Code and Data](https://github.com/facebookresearch/cruxeval)
  - **Code Simulation Challenges** — Tests straight-line execution, critical paths, redundant computation, loops, and sorting. [Paper](https://arxiv.org/abs/2401.09074) · [Code and Data](https://github.com/EmanueleLM/CodeSimulation)

## World Models as Code

These works use executable code as the **representation of another world's dynamics**. They are highly relevant to the name “Code World Model,” but are separated because the modeled world is a game or general environment rather than software execution.

- **Code World Models for General Game Playing** — ICLR 2026. Generates executable Python game simulators from natural-language specifications and uses them for model-based planning.
  [Paper](https://proceedings.iclr.cc/paper_files/paper/2026/hash/d8a12fde9e72444e1b356e8c37e53753-Abstract-Conference.html)

- **Generating Code World Models with Large Language Models Guided by Monte Carlo Tree Search** — NeurIPS 2024. Searches over executable simulator programs and evaluates them through downstream gameplay.
  [Paper](https://proceedings.neurips.cc/paper_files/paper/2024/hash/6f479ea488e0908ac8b1b37b27fd134c-Abstract-Conference.html) · [Code](https://github.com/nicoladainese96/code-world-models)

- **WorldCoder, a Model-Based LLM Agent: Building World Models by Writing Code and Interacting with the Environment** — NeurIPS 2024. Induces executable environment models from interactions and plans inside the synthesized simulators.
  [Paper](https://proceedings.neurips.cc/paper_files/paper/2024/hash/820c61a0cd419163ccbd2c33b268816e-Abstract-Conference.html) · [Code](https://github.com/haotang1995/WorldCoder)

## Contributing

Contributions are welcome. Before opening a pull request, please check that the resource:

1. Predicts an execution-relevant state transition or outcome from code/environment context, **or** directly evaluates that capability.
2. Is not only a general code LLM, coding agent, static analyzer, or wrapper around a real executor.
3. Links to a canonical paper/project page and, when available, public code, data, and model weights.
4. Includes one sentence explaining why it belongs in this collection.

Please keep entries reverse chronological within each section and prefer archival conference pages over secondary summaries.

## Acknowledgements

The organization of this repository is inspired by [Awesome World Models](https://github.com/knightnemo/Awesome-World-Models) and [Awesome WAM](https://github.com/OpenMOSS/Awesome-WAM). Thanks to their maintainers and to everyone building open resources for world-model research.

If you find a missing paper or a classification mistake, please open an issue or submit a pull request.
