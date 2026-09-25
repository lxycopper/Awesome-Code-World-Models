# Awesome Code World Models

[![Awesome](https://awesome.re/badge.svg)](https://awesome.re)
![Last updated](https://img.shields.io/badge/last%20updated-September%202026-blue)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](#contributing)

A curated collection of papers, models, datasets, benchmarks, and resources on **Code World Models (CWMs)**—world models whose state and dynamics are represented, generated, or maintained as executable code.

> **Primary focus:** code as the persistent, programmable “brain” of an external visual, physical, or interactive world. Software-execution world models are retained as a separate second direction because the term CWM is also used there.

## What is a Code World Model?

A **Code World Model** represents an external world's entities, state, relations, events, and transition rules as an executable program. Given the current authoritative state $s_t$, an action $a_t$, and optional events $e_t$, executing the program advances the world:

$$
s_{t+1} = P(s_t, a_t, e_t)
$$

Here, $P$ is the executable world program. It may be written by a coding agent, induced from observations and interactions, synthesized from a specification, or manually authored. In visual CWMs, a compiler converts the updated state into spatial controls—such as 3D boxes, proxy geometry, trajectories, semantic IDs, and camera paths—and a generative renderer turns those controls into video observations. This separation lets code preserve exact, persistent, off-screen, and non-visual facts while a neural model focuses on appearance.

### Inclusion Criteria

- Executable state and rule systems coupled to neural image or video renderers.
- World models learned or generated as code, probabilistic programs, PDDL, or simulator programs.
- Coding agents that infer, edit, verify, and plan with executable world representations.
- Explicit-state world models that clarify the state/dynamics/rendering decomposition.
- Software-execution world models, kept in their own clearly labeled direction.

### Exclusion Criteria

- Ordinary code generation without an executable model of world dynamics.
- Generic game engines or simulators that are not generated, induced, or part of a world-model contribution.
- Pixel-only or latent video world models with no explicit programmatic state, except as contextual related work.
- Coding agents that neither construct nor use a world model.

<p align="center">
  <img src="assets/code-world-model-overview.png" width="100%" alt="A coding agent writes an executable world program, a state engine maintains persistent world state, a compiler creates visual controls, and a generative video model renders observations." />
</p>

## Contents

- [Taxonomy at a Glance](#taxonomy-at-a-glance)
- [How to Compare These Works](#how-to-compare-these-works)
- [Work Comparison](#work-comparison)
  - [Programmatic Visual Models](#programmatic-visual-models)
  - [Explicit-State and Rendering Models](#explicit-state-and-rendering-models)
  - [Executable Program Models](#executable-program-models)
- [Programmatic Visual World Models](#programmatic-visual-world-models)
- [Explicit-State Models and Generative Renderers](#explicit-state-models-and-generative-renderers)
- [World Models as Executable Programs](#world-models-as-executable-programs)
- [Programmatic-World Benchmarks](#programmatic-world-benchmarks)
- [Adjacent Neural Visual World Models](#adjacent-neural-visual-world-models)
- [Software World Models](#software-world-models)
  - [Software Model Comparison](#software-model-comparison)
  - [Repository and Terminal World Models](#repository-and-terminal-world-models)
  - [Program Execution and State Models](#program-execution-and-state-models)
  - [Software-World Evaluation and Analysis](#software-world-evaluation-and-analysis)
  - [Software-World Benchmarks](#software-world-benchmarks)
- [Benchmark Selection Guide](#benchmark-selection-guide)
- [Reading Paths](#reading-paths)
- [Research Questions](#research-questions)
- [Terminology and Scope](#terminology-and-scope)
- [Contributing](#contributing)

## Tag Legend

- ![Code State][tag-code-state] — code is the authoritative world-state representation
- ![Agent Written][tag-agent-written] — a coding agent creates or edits world programs
- ![Induced][tag-induced] — executable dynamics are learned from observations or interaction
- ![Neural Renderer][tag-neural-renderer] — a generative model realizes visual observations from structured controls
- ![Explicit State][tag-explicit-state] — world or game state is explicitly represented alongside observations
- ![Software World][tag-software-world] — predicts the behavior of programs or software environments
- ![Transition][tag-transition] / ![Trace][tag-trace] / ![Outcome][tag-outcome] / ![Reward][tag-reward] — software-world prediction targets
- ![Repository][tag-repo] / ![Terminal][tag-terminal] — repository-scale or command-line software environments

## Taxonomy at a Glance

The poster separates the repository's primary focus—code as the executable representation of an external world—from software world models, where code is the world being modeled. The dashed card marks adjacent neural visual models that provide rendering context without executable state.

<p align="center">
  <img src="assets/code-world-model-taxonomy.svg" width="100%" alt="Taxonomy of Code World Models, including programmatic visual world models, executable program world models, explicit-state bridges, software world models, benchmarks, and adjacent neural visual models." />
</p>

## How to Compare These Works

The most useful comparison starts with **what is represented and what is executed**. An executable simulator of an external world, a learned predictor of explicit state, and a language model that predicts software execution serve different roles, even when all three are called world models.

| Dimension | What to look for | Why it matters |
| --- | --- | --- |
| Modeled world | Physical scenes, interactive games, or software environments | Establishes which systems address the same problem. |
| State representation | Program variables, object states, belief states, execution traces, or implicit features | Identifies what can be inspected, edited, and preserved across steps. |
| Source of dynamics | Authored rules, language specifications, offline trajectories, or online interaction | Distinguishes implementing supplied rules from discovering unknown rules. |
| Model construction | Program synthesis, search and repair, probabilistic induction, or neural training | Clarifies what changes when new evidence arrives. |
| Observation generation | Simulator rendering, structured controls plus neural rendering, or predicted tool output | Separates transition correctness from observation quality. |
| Validation | State fidelity, rollout consistency, planning success, or execution accuracy | Connects the claimed capability to a suitable evaluation. |

For example, [PWM](https://arxiv.org/abs/2609.10540) executes generated rules to maintain a persistent world and then renders it, while [GIF-MCTS](https://arxiv.org/abs/2405.15383) searches for simulator code consistent with descriptions and collected trajectories. These are different sources of dynamics: executability alone does not establish that a model has discovered the world's rules.

## Work Comparison

These tables summarize the external-world methods already listed below. Work names link to primary sources; the original entries retain full titles and resource links. The tables compare system designs, not scores from different evaluation settings. Years follow the citations in the paper list.

### Programmatic Visual Models

| Work | World representation | How dynamics are obtained | Observation / rendering | Main use or evaluation focus |
| --- | --- | --- | --- | --- |
| [CoDeR: Code Plans, Diffusion Renders (2026)](https://arxiv.org/abs/2609.26458) | Modular executable 3D worlds with registered appearance | Coding agents implement shared rules and interacting systems | Whitebox video, depth and prompts condition MiniMax-H3 | Persistent memory, open-ended interactions and autonomous multi-agent evolution |
| [Programmable World Model (2026)](https://arxiv.org/abs/2609.10540) | Persistent entity state with state-augmented 3D boxes | Agent-written transition programs run in a state engine | Deterministically compiled spatial controls condition a video model | CombatStateBench: entity counts and persistent state over long horizons |
| [Code as Worlds (2026)](https://arxiv.org/abs/2608.27549) | Executable physical composition, evolution and appearance specifications | Agent refines hypotheses from text or video evidence | MuJoCo simulation, followed by neural sim-to-real re-rendering | Physical reconstruction and quantitative reasoning supervision; QuantiPhy |
| [Coding Agent as World Brain (2026)](https://arxiv.org/abs/2608.25927) | Persistent executable entity states and world rules | Coding agent reasons about events and updates world programs | Compiled proxy videos condition fine-tuned MiniMax-H3 | Rule-consistent interactive worlds with controllable visual realizations |
| [VisPhyWorld (2026)](https://arxiv.org/abs/2602.13294) | Executable 2D/3D scene and physics hypotheses | Multimodal models infer simulation code from visual keyframes | Simulator rendering, primarily Three.js and p5.js | VisPhyBench: visual reconstruction and physical motion fidelity |

### Explicit-State and Rendering Models

These are related explicit-state systems; the table distinguishes separate state/rendering engines from joint state-and-video prediction.

| Work | World representation | How dynamics are obtained | Observation / rendering | Main use or evaluation focus |
| --- | --- | --- | --- | --- |
| [Magpie (2026)](https://arxiv.org/abs/2608.27168) | Game-engine state, scene geometry and authored rules | Conventional engine executes designer-defined mechanics and player actions | Independent generative server renders engine-produced whitebox frames | Real-time interactive game rendering and asset-light prototyping |
| [Marionette (2026)](https://arxiv.org/abs/2608.14530) | Explicit articulated 3D poses, root trajectories and rotations | Learned two-stage autoregressive state dynamics; optional rule corrections | Fixed geometry bridge produces controls for video diffusion | Action control and long-horizon repair in articulated-character games |
| [MASS (2026)](https://arxiv.org/abs/2608.06257) | Global typed state shared by all players | Learned logic engine predicts transitions from joint actions | Learned rendering engine produces camera-specific views on demand | Multiplayer Snake: state accuracy, cross-view consistency and scalability |
| [StatePlay (2026)](https://arxiv.org/abs/2607.26754) | Explicit game variables coupled with visual representations | Jointly learned state and video prediction with interacting branches | Predicted game states guide the visual-generation branch | State prediction and mechanics fidelity in generated gameplay |

### Executable Program Models

| Work | Evidence / input | Executable representation | Construction / update mechanism | Main use |
| --- | --- | --- | --- | --- |
| [VisualPatchWorld (2026)](https://arxiv.org/abs/2607.25236) | Active probes and offline structured state–action traces | Python transition programs over structured scene graphs | Select dynamics sketches, then fit multi-step rollout parameters | Model-predictive control with image-based state estimation |
| [Mind-Studio (2026)](https://arxiv.org/abs/2606.16070) | Logged object-level Atari transitions and scene information | Standalone Python object dynamics and scene rendering | Entropy-based trace selection, compression, and skill-guided LLM synthesis | Lookahead action selection in partially observable Atari |
| [ARC-AGI-3 executable models (2026)](https://arxiv.org/abs/2605.05138) | Online game observations and recorded action outcomes | Persistent Python dynamics, state reconstruction, and rendering | Coding-agent edits, observation replay verification, and model refactoring | Planning and checking actions in interactive puzzle games |
| [PatchWorld (2026)](https://arxiv.org/abs/2605.30880) | Offline text observation–action trajectories under partial observability | Python belief state, transitions, correction, and observation readout | Counterexample-guided patches accepted through held-out validation replay | Observation prediction and lookahead for text agents |
| [General Game Playing CWMs (2026)](https://proceedings.iclr.cc/paper_files/paper/2026/hash/d8a12fde9e72444e1b356e8c37e53753-Abstract-Conference.html) | Natural-language game rules and offline game trajectories | Python game simulator with optional hidden-state inference functions | LLM synthesis and refinement using trajectory-derived unit tests | MCTS for perfect- and imperfect-information game playing |
| [PoE-World (2025)](https://proceedings.neurips.cc/paper_files/paper/2025/hash/262dd62fd1bbb30d6a6b4d578f5e65ff-Abstract-Conference.html) | Brief object-level demonstrations followed by online interaction | Weighted product of Python experts predicting observation distributions | Synthesize experts, fit weights, and prune after new observations | Planning and policy learning in partially observable Atari |
| [POMDP Coder (2025)](https://arxiv.org/abs/2505.02216) | Demonstrations and interaction; states revealed after episodes | Probabilistic initial-state, transition, observation, and reward programs | LLM proposals and coverage-guided repair between episodes | Belief-space planning in games and robot search |
| [FactorSim (2024)](https://proceedings.neurips.cc/paper_files/paper/2024/hash/9f35ec2f7f403ef2c83d65b581df10bc-Abstract-Conference.html) | Natural-language task descriptions or environment documentation | Complete simulation code organized as a factored POMDP | Decompose requirements and generate modules with selected state context | Train policies in generated game and robotics simulations |
| [GIF-MCTS (2024)](https://arxiv.org/abs/2405.15383) | Natural-language environment descriptions and collected offline trajectories | Python simulator predicting next state, reward, and termination | Search generate, improve, and fix actions using transition tests | Model-based planning across discrete and continuous control tasks |
| [WorldCoder (2024)](https://proceedings.neurips.cc/paper_files/paper/2024/hash/820c61a0cd419163ccbd2c33b268816e-Abstract-Conference.html) | State transitions and rewards collected through online interaction | Python programs for state transitions and rewards | Synthesize and repair models under experience and optimism constraints | Exploration and planning in gridworlds and household tasks |

## Programmatic Visual World Models

These works most directly match this repository's primary focus: executable programs maintain or reconstruct a world, and visual observations are produced through a rendering pipeline.

- <div><strong>Programmable World Model</strong>. arXiv 2026.<br>
  <a href="https://arxiv.org/abs/2609.10540"><img src="https://img.shields.io/badge/arXiv-Paper-B31B1B?logo=arxiv&logoColor=white" alt="arXiv"></a> <a href="https://alaya-lab.github.io/pwm/"><img src="https://img.shields.io/badge/Website-Link-2563EB" alt="Website"></a> <a href="https://github.com/AlayaLab/pwm"><img src="https://img.shields.io/badge/Repository-GitHub-94A3B8?logo=github&logoColor=white" alt="Repository"></a></div>
  <details>
  <summary>TL;DR</summary>

  A coding agent translates instructions into entity states and executable transition rules; a lightweight engine maintains persistent off-screen and non-visual state; state-augmented 3D oriented bounding boxes are compiled into controls for a pretrained video renderer. Introduces CombatStateBench.

  ![Code State][tag-code-state] ![Agent Written][tag-agent-written] ![Neural Renderer][tag-neural-renderer]

  </details>

- <div><strong>Code World Model: Coding Agent as World Brain</strong>. arXiv 2026.<br>
  <a href="https://arxiv.org/abs/2608.25927"><img src="https://img.shields.io/badge/arXiv-Paper-B31B1B?logo=arxiv&logoColor=white" alt="arXiv"></a> <a href="https://buaacyw.github.io/cwm/"><img src="https://img.shields.io/badge/Website-Link-2563EB" alt="Website"></a> <a href="https://github.com/buaacyw/code-world-model"><img src="https://img.shields.io/badge/Code-GitHub-2DA44E?logo=github&logoColor=white" alt="Code"></a> <a href="https://huggingface.co/NTU-yiwen/awm-minimax-h3-new1344-lora-checkpoints"><img src="https://img.shields.io/badge/Model-Hugging_Face-FFD21E" alt="Model"></a></div>
  <details>
  <summary>TL;DR</summary>

  A coding agent continually creates and updates executable world state and rules, compiles them into proxy videos and structured prompts, and conditions MiniMax-H3 to render open-ended visual observations.

  ![Code State][tag-code-state] ![Agent Written][tag-agent-written] ![Neural Renderer][tag-neural-renderer]

  </details>

- <div><strong>Code as Worlds: Agentic Discovery of Executable World Representations for Physical Reasoning</strong>. arXiv 2026.<br>
  <a href="https://arxiv.org/abs/2608.27549"><img src="https://img.shields.io/badge/arXiv-Paper-B31B1B?logo=arxiv&logoColor=white" alt="arXiv"></a> <a href="https://mirros-lab.github.io/code-as-world/"><img src="https://img.shields.io/badge/Website-Link-2563EB" alt="Website"></a> <a href="https://github.com/MirroS-Lab/Code-as-World"><img src="https://img.shields.io/badge/Code-GitHub-2DA44E?logo=github&logoColor=white" alt="Code"></a></div>
  <details>
  <summary>TL;DR</summary>

  Represents physical composition, dynamics, and appearance as executable code, discovered through a propose–execute–render–verify loop from text or video evidence; simulated videos can then be re-rendered by a neural video model.

  ![Code State][tag-code-state] ![Agent Written][tag-agent-written] ![Induced][tag-induced] ![Neural Renderer][tag-neural-renderer]

  </details>

- <div><strong>VisPhyWorld: Probing Physical Reasoning via Code-Driven Video Reconstruction</strong>. arXiv 2026.<br>
  <a href="https://arxiv.org/abs/2602.13294"><img src="https://img.shields.io/badge/arXiv-Paper-B31B1B?logo=arxiv&logoColor=white" alt="arXiv"></a> <a href="https://tiger-ai-lab.github.io/VisPhyWorld/"><img src="https://img.shields.io/badge/Website-Link-2563EB" alt="Website"></a> <a href="https://github.com/TIGER-AI-Lab/VisPhyWorld"><img src="https://img.shields.io/badge/Code-GitHub-2DA44E?logo=github&logoColor=white" alt="Code"></a></div>
  <details>
  <summary>TL;DR</summary>

  Requires multimodal models to infer executable 2D/3D physics simulation code from visual keyframes and evaluates reconstructed appearance and physical motion, making the inferred dynamics inspectable and falsifiable.

  ![Code State][tag-code-state] ![Agent Written][tag-agent-written] ![Induced][tag-induced]

  </details>

- <div><strong>Code Plans, Diffusion Renders: Open-Ended Generative World Modeling</strong>. arXiv 2026.<br>
  <a href="https://arxiv.org/abs/2609.26458"><img src="https://img.shields.io/badge/arXiv-Paper-B31B1B?logo=arxiv&logoColor=white" alt="arXiv"></a></div>
  <details>
  <summary>TL;DR</summary>

  Coordinates five complementary roles to translate high-level concepts into structured world rules, executable dynamics, and perceptual observations, enabling long-term memory, open-ended interactions, autonomous world evolution, and multi-agent scenarios.

  ![Code State][tag-code-state] ![Agent Written][tag-agent-written] ![Neural Renderer][tag-neural-renderer]

  </details>

## Explicit-State Models and Generative Renderers

These systems expose explicit world or game state alongside neural visual generation. Dynamics may be learned or conventionally authored, and the coupling ranges from separate state/rendering engines to joint state-and-video prediction.

- <div><strong>Magpie: Real-Time World Renderer for Interactive Games</strong>. arXiv 2026.<br>
  <a href="https://arxiv.org/abs/2608.27168"><img src="https://img.shields.io/badge/arXiv-Paper-B31B1B?logo=arxiv&logoColor=white" alt="arXiv"></a> <a href="https://zhanxy.xyz/Magpie-website"><img src="https://img.shields.io/badge/Website-Link-2563EB" alt="Website"></a> <a href="https://huggingface.co/datasets/MogoAI/Magpie_lite"><img src="https://img.shields.io/badge/Data-Dataset-F59E0B" alt="Data"></a></div>
  <details>
  <summary>TL;DR</summary>

  A conventional game engine owns rules and state while a separate generative render server converts white-box frames into high-fidelity real-time video.

  ![Explicit State][tag-explicit-state] ![Neural Renderer][tag-neural-renderer]

  </details>

- <div><strong>Marionette: Predicting World States, Rendering Geometry, Painting Appearance</strong>. arXiv 2026.<br>
  <a href="https://arxiv.org/abs/2608.14530"><img src="https://img.shields.io/badge/arXiv-Paper-B31B1B?logo=arxiv&logoColor=white" alt="arXiv"></a> <a href="https://alayalab.github.io/Marionette/"><img src="https://img.shields.io/badge/Website-Link-2563EB" alt="Website"></a> <a href="https://github.com/AlayaLab/Marionette"><img src="https://img.shields.io/badge/Code-GitHub-2DA44E?logo=github&logoColor=white" alt="Code"></a> <a href="https://huggingface.co/AlayaLab/Marionette"><img src="https://img.shields.io/badge/Model-Hugging_Face-FFD21E" alt="Model"></a></div>
  <details>
  <summary>TL;DR</summary>

  Predicts explicit articulated 3D state, deterministically converts it into pose-control video, and leaves only appearance synthesis to video diffusion; state-level rules can directly repair rollouts.

  ![Explicit State][tag-explicit-state] ![Neural Renderer][tag-neural-renderer]

  </details>

- <div><strong>MASS: Multiplayer World Models with Authoritative Shared State</strong>. arXiv 2026.<br>
  <a href="https://arxiv.org/abs/2608.06257"><img src="https://img.shields.io/badge/arXiv-Paper-B31B1B?logo=arxiv&logoColor=white" alt="arXiv"></a> <a href="https://alaya-lab.github.io/MASS/"><img src="https://img.shields.io/badge/Website-Link-2563EB" alt="Website"></a></div>
  <details>
  <summary>TL;DR</summary>

  A learned logic engine advances a global typed state, while independent neural renderers produce consistent player-specific views.

  ![Explicit State][tag-explicit-state] ![Neural Renderer][tag-neural-renderer]

  </details>

- <div><strong>StatePlay: State-Aware Game World Models for Mechanics-Consistent Generation</strong>. arXiv 2026.<br>
  <a href="https://arxiv.org/abs/2607.26754"><img src="https://img.shields.io/badge/arXiv-Paper-B31B1B?logo=arxiv&logoColor=white" alt="arXiv"></a> <a href="https://jimntu.github.io/stateplay_page/"><img src="https://img.shields.io/badge/Website-Link-2563EB" alt="Website"></a> <a href="https://github.com/Jimntu/StatePlay"><img src="https://img.shields.io/badge/Code-GitHub-2DA44E?logo=github&logoColor=white" alt="Code"></a> <a href="https://huggingface.co/onepiece1999/StatePlay"><img src="https://img.shields.io/badge/Model-Hugging_Face-FFD21E" alt="Model"></a></div>
  <details>
  <summary>TL;DR</summary>

  Jointly predicts explicit game variables and visual content so mechanics such as health, timers, and skill meters constrain generated gameplay.

  ![Explicit State][tag-explicit-state] ![Neural Renderer][tag-neural-renderer]

  </details>

## World Models as Executable Programs

These works learn, synthesize, or repair executable transition models for planning and simulation, usually without a neural visual renderer.

- <div><strong>VisualPatchWorld: Code World Models as Latent Structured Representations for Planning</strong>. arXiv 2026.<br>
  <a href="https://arxiv.org/abs/2607.25236"><img src="https://img.shields.io/badge/arXiv-Paper-B31B1B?logo=arxiv&logoColor=white" alt="arXiv"></a> <a href="https://github.com/HKBU-KnowComp/VisualPatchWorld"><img src="https://img.shields.io/badge/Code-GitHub-2DA44E?logo=github&logoColor=white" alt="Code"></a></div>
  <details>
  <summary>TL;DR</summary>

  Selects executable dynamics sketches through active probes and fits their parameters from offline structured state–action traces; image-derived scene graphs supply live state for model-predictive control.

  ![Induced][tag-induced]

  </details>

- <div><strong>Mind-Studio: Executable World Models with Lookahead Evaluation for Partially Observable Games</strong>. arXiv 2026.<br>
  <a href="https://arxiv.org/abs/2606.16070"><img src="https://img.shields.io/badge/arXiv-Paper-B31B1B?logo=arxiv&logoColor=white" alt="arXiv"></a> <a href="https://github.com/HKBU-KnowComp/MindStudio"><img src="https://img.shields.io/badge/Code-GitHub-2DA44E?logo=github&logoColor=white" alt="Code"></a></div>
  <details>
  <summary>TL;DR</summary>

  Synthesizes standalone transition-and-render programs for partially observable Atari games and uses them for lookahead evaluation.

  ![Agent Written][tag-agent-written] ![Induced][tag-induced]

  </details>

- <div><strong>Executable World Models for ARC-AGI-3 in the Era of Coding Agents</strong>. arXiv 2026.<br>
  <a href="https://arxiv.org/abs/2605.05138"><img src="https://img.shields.io/badge/arXiv-Paper-B31B1B?logo=arxiv&logoColor=white" alt="arXiv"></a> <a href="https://github.com/astroseger/arc-3-agents-baseline1"><img src="https://img.shields.io/badge/Code-GitHub-2DA44E?logo=github&logoColor=white" alt="Code"></a></div>
  <details>
  <summary>TL;DR</summary>

  A coding agent maintains, verifies, simplifies, and plans through persistent Python models of interactive games.

  ![Agent Written][tag-agent-written] ![Induced][tag-induced]

  </details>

- <div><strong>PatchWorld: Gradient-Free Optimization of Executable World Models for Agent Environments</strong>. arXiv 2026.<br>
  <a href="https://arxiv.org/abs/2605.30880"><img src="https://img.shields.io/badge/arXiv-Paper-B31B1B?logo=arxiv&logoColor=white" alt="arXiv"></a> <a href="https://github.com/HKBU-KnowComp/PatchWorld"><img src="https://img.shields.io/badge/Code-GitHub-2DA44E?logo=github&logoColor=white" alt="Code"></a></div>
  <details>
  <summary>TL;DR</summary>

  Induces persistent Python belief-state programs from offline trajectories through counterexample-guided repair.

  ![Induced][tag-induced]

  </details>

- <div><strong>Code World Models for General Game Playing</strong>. ICLR 2026.<br>
  <a href="https://proceedings.iclr.cc/paper_files/paper/2026/hash/d8a12fde9e72444e1b356e8c37e53753-Abstract-Conference.html"><img src="https://img.shields.io/badge/Paper-Link-555555" alt="Paper"></a></div>
  <details>
  <summary>TL;DR</summary>

  Compiles natural-language game rules and demonstrations into executable Python functions for transitions, legal actions, observations, rewards, and termination, then plans with MCTS.

  ![Agent Written][tag-agent-written]

  </details>

- <div><strong>PoE-World: Compositional World Modeling with Products of Programmatic Experts</strong>. NeurIPS 2025 Spotlight.<br>
  <a href="https://proceedings.neurips.cc/paper_files/paper/2025/hash/262dd62fd1bbb30d6a6b4d578f5e65ff-Abstract-Conference.html"><img src="https://img.shields.io/badge/Paper-Link-555555" alt="Paper"></a> <a href="https://topwasu.github.io/poe-world"><img src="https://img.shields.io/badge/Website-Link-2563EB" alt="Website"></a> <a href="https://github.com/topwasu/poe-world"><img src="https://img.shields.io/badge/Code-GitHub-2DA44E?logo=github&logoColor=white" alt="Code"></a></div>
  <details>
  <summary>TL;DR</summary>

  Learns stochastic, partially observable Atari dynamics as a weighted product of small LLM-synthesized Python programs.

  ![Agent Written][tag-agent-written] ![Induced][tag-induced]

  </details>

- <div><strong>LLM-Guided Probabilistic Program Induction for POMDP Model Estimation</strong>. 2025.<br>
  <a href="https://arxiv.org/abs/2505.02216"><img src="https://img.shields.io/badge/arXiv-Paper-B31B1B?logo=arxiv&logoColor=white" alt="arXiv"></a></div>
  <details>
  <summary>TL;DR</summary>

  Generates probabilistic programs for initial state, transition, observation, and reward functions under partial observability.

  ![Agent Written][tag-agent-written] ![Induced][tag-induced]

  </details>

- <div><strong>FactorSim: Generative Simulation via Factorized Representation</strong>. NeurIPS 2024.<br>
  <a href="https://proceedings.neurips.cc/paper_files/paper/2024/hash/9f35ec2f7f403ef2c83d65b581df10bc-Abstract-Conference.html"><img src="https://img.shields.io/badge/Paper-Link-555555" alt="Paper"></a> <a href="https://cs.stanford.edu/~sunfanyun/factorsim/"><img src="https://img.shields.io/badge/Website-Link-2563EB" alt="Website"></a> <a href="https://github.com/sunfanyunn/FactorSim"><img src="https://img.shields.io/badge/Code-GitHub-2DA44E?logo=github&logoColor=white" alt="Code"></a></div>
  <details>
  <summary>TL;DR</summary>

  Generates complete game and robotics simulations from language using a factorized POMDP representation.

  ![Agent Written][tag-agent-written]

  </details>

- <div><strong>Generating Code World Models with Large Language Models Guided by Monte Carlo Tree Search</strong>. NeurIPS 2024.<br>
  <a href="https://proceedings.neurips.cc/paper_files/paper/2024/hash/6f479ea488e0908ac8b1b37b27fd134c-Abstract-Conference.html"><img src="https://img.shields.io/badge/Paper-Link-555555" alt="Paper"></a> <a href="https://sites.google.com/view/code-world-models/home"><img src="https://img.shields.io/badge/Website-Link-2563EB" alt="Website"></a> <a href="https://github.com/nicoladainese96/code-world-models"><img src="https://img.shields.io/badge/Code-GitHub-2DA44E?logo=github&logoColor=white" alt="Code"></a></div>
  <details>
  <summary>TL;DR</summary>

  Uses Monte Carlo tree search to generate, improve, and fix Python simulator programs against natural-language descriptions and offline trajectory tests, then evaluates them through model-based planning; introduces the Code World Models Benchmark (CWMB).

  ![Agent Written][tag-agent-written] ![Induced][tag-induced]

  </details>

- <div><strong>WorldCoder, a Model-Based LLM Agent: Building World Models by Writing Code and Interacting with the Environment</strong>. NeurIPS 2024.<br>
  <a href="https://proceedings.neurips.cc/paper_files/paper/2024/hash/820c61a0cd419163ccbd2c33b268816e-Abstract-Conference.html"><img src="https://img.shields.io/badge/Paper-Link-555555" alt="Paper"></a> <a href="https://haotang1995.github.io/projects/worldcoder"><img src="https://img.shields.io/badge/Website-Link-2563EB" alt="Website"></a> <a href="https://github.com/haotang1995/WorldCoder"><img src="https://img.shields.io/badge/Code-GitHub-2DA44E?logo=github&logoColor=white" alt="Code"></a></div>
  <details>
  <summary>TL;DR</summary>

  Induces executable transition and reward programs from interaction, verifies them against experience, and plans inside the learned model.

  ![Agent Written][tag-agent-written] ![Induced][tag-induced]

  </details>

## Programmatic-World Benchmarks

- **CombatStateBench** — Long-horizon entity count and persistent-state evaluation with moving cameras, off-screen entities, and irreversible events. [![arXiv][res-arxiv]](https://arxiv.org/abs/2609.10540) [![Website][res-website]](https://alaya-lab.github.io/pwm/)
- **VisPhyBench** — 209 scenes from 108 physical templates for evaluating code-driven physical reconstruction and re-simulation. [![arXiv][res-arxiv]](https://arxiv.org/abs/2602.13294) [![Code and Data][res-code-data]](https://github.com/TIGER-AI-Lab/VisPhyWorld)
- **CWMB** — 18 environments for evaluating induced executable transition models through model fidelity and downstream policy learning. [![arXiv][res-arxiv]](https://arxiv.org/abs/2405.15383) [![Code and Data][res-code-data]](https://github.com/nicoladainese96/code-world-models)
- **Text2World** — Generation of executable PDDL world models from natural-language descriptions. [![Paper][res-paper]](https://aclanthology.org/2025.findings-acl.1337/) [![Code and Data][res-code-data]](https://github.com/Aaron617/text2world)

## Adjacent Neural Visual World Models

These representative interactive video world models provide important rendering and action-conditioning context, but do not use an authoritative executable program as world state.

- **YUME** — *YUME: An Interactive World Generation Model*. arXiv 2025. [![arXiv][res-arxiv]](https://arxiv.org/abs/2507.17744)
- **GameNGen** — *Diffusion Models Are Real-Time Game Engines*. arXiv 2024. [![arXiv][res-arxiv]](https://arxiv.org/abs/2408.14837) [![Website][res-website]](https://gamengen.github.io/)
- **DIAMOND** — *Diffusion for World Modeling: Visual Details Matter in Atari*. NeurIPS 2024. [![arXiv][res-arxiv]](https://arxiv.org/abs/2405.12399) [![Code][res-code]](https://github.com/eloialonso/diamond)
- **Genie** — *Genie: Generative Interactive Environments*. ICML 2024. [![arXiv][res-arxiv]](https://arxiv.org/abs/2402.15391) [![Website][res-website]](https://sites.google.com/view/genie-2024/)

## Software World Models

This second direction uses “Code World Model” to mean a learned model of **software execution**: how code, commands, tools, repositories, and tests change a computational environment. Here, code is the modeled world rather than the representation of an external world.

### Software Model Comparison

Here, a learned model predicts the effects of executing code or taking software actions. Predicting a trace or tool response is a different capability from constructing an executable simulator of an external world. The table covers the model papers below; evaluation and analysis resources remain in their own sections.

| Work | Modeled environment | Prediction target | Training signal / use |
| --- | --- | --- | --- |
| [Qwen-AgentWorld (2026)](https://arxiv.org/abs/2606.24597) | Agent environments, including terminal and software-engineering domains | Next observations following agent actions | Interaction trajectories; pretraining, SFT and RL for agent simulation |
| [ECHO (2026)](https://arxiv.org/abs/2605.24517) | Command-line environments encountered during agent rollouts | Environment-response tokens produced by the policy's actions | Auxiliary observation-prediction loss alongside policy-gradient training |
| [SWE-World (2026)](https://arxiv.org/abs/2602.03419) | Repository-level software-engineering environments | Intermediate command outcomes and final test feedback | Real interaction data; surrogate agent training and solution selection |
| [Meta CWM (2025)](https://arxiv.org/abs/2510.02387) | Python interpreter and agentic Docker environments | Python execution states and software-environment responses | Trajectory mid-training and reasoning RL for coding tasks |
| [ExecVerify (2026)](https://aclanthology.org/2026.acl-long.631/) | Synthesized programs and code-execution reasoning tasks | Next statement, variable values/types and final outputs | Verifiable stepwise RL rewards; transfer to code generation |
| [Self-Execution Simulation (2026)](https://arxiv.org/abs/2604.03253) | Competitive-programming solutions evaluated on test inputs | Execution explanations and predicted test outputs | Execution-grounded SFT and verifiable RL; self-verification and repair |
| [Towards a Neural Debugger for Python (2026)](https://arxiv.org/abs/2603.09951) | Python execution controlled by debugger actions | Forward states/outputs and inverse prior states/inputs | Debugger-conditioned training; controllable execution reasoning |
| [Execution Tuning (2025)](https://arxiv.org/abs/2503.05703) | Real-world program execution at multiple granularities | Line/instruction-level traces and program outputs | Execution-trace training; dynamic scratchpads for long execution |
| [SemCoder (2024)](https://proceedings.neurips.cc/paper_files/paper/2024/hash/6efcc7fd8efeee29a050a79c843c90e0-Abstract-Conference.html) | Python programs with descriptions and test cases | Semantic monologues about execution effects and input/output behavior | Natural-language semantics training; code generation, reasoning and repair |
| [TRACED (2024)](https://arxiv.org/abs/2306.07487) | C programs paired with executable inputs | Execution coverage and quantized runtime variable states | Trace-aware pretraining; execution estimation, clone retrieval and vulnerability detection |
| [CodeExecutor (2023)](https://aclanthology.org/2023.findings-acl.308/) | Python programs with mutation-augmented execution data | Line-by-line execution traces and final outputs | Execution pretraining and curriculum learning; code-intelligence transfer |
| [Learning to Execute (2014)](https://arxiv.org/abs/1410.4615) | Short synthetic programs with restricted computational structure | Program outputs directly from character-level source | Supervised LSTM training with a tailored curriculum |

### Repository and Terminal World Models

- <div><strong>Qwen-AgentWorld: Language World Models for General Agents</strong>. arXiv 2026.<br>
  <a href="https://arxiv.org/abs/2606.24597"><img src="https://img.shields.io/badge/arXiv-Paper-B31B1B?logo=arxiv&logoColor=white" alt="arXiv"></a> <a href="https://github.com/QwenLM/Qwen-AgentWorld"><img src="https://img.shields.io/badge/Code-GitHub-2DA44E?logo=github&logoColor=white" alt="Code"></a> <a href="https://huggingface.co/Qwen/Qwen-AgentWorld-35B-A3B"><img src="https://img.shields.io/badge/Model-Hugging_Face-FFD21E" alt="Model"></a> <a href="https://huggingface.co/datasets/Qwen/AgentWorldBench"><img src="https://img.shields.io/badge/Benchmark-Dataset-F97316" alt="Benchmark"></a></div>
  <details>
  <summary>TL;DR</summary>

  A native next-observation model trained across seven agent environments; its Terminal and SWE domains predict shell, file-system, edit, compiler, and test feedback.

  ![Transition][tag-transition] ![Repository][tag-repo] ![Terminal][tag-terminal]

  </details>

- <div><strong>ECHO: Terminal Agents Learn World Models for Free</strong>. arXiv 2026.<br>
  <a href="https://arxiv.org/abs/2605.24517"><img src="https://img.shields.io/badge/arXiv-Paper-B31B1B?logo=arxiv&logoColor=white" alt="arXiv"></a></div>
  <details>
  <summary>TL;DR</summary>

  Adds an environment-observation prediction objective to terminal-agent RL, learning command effects from stdout, errors, files, logs, and traces already present in rollouts.

  ![Transition][tag-transition] ![Terminal][tag-terminal]

  </details>

- <div><strong>SWE-World: Building Software Engineering Agents in Docker-Free Environments</strong>. arXiv 2026.<br>
  <a href="https://arxiv.org/abs/2602.03419"><img src="https://img.shields.io/badge/arXiv-Paper-B31B1B?logo=arxiv&logoColor=white" alt="arXiv"></a> <a href="https://github.com/RUCAIBox/SWE-World"><img src="https://img.shields.io/badge/Code-GitHub-2DA44E?logo=github&logoColor=white" alt="Code"></a></div>
  <details>
  <summary>TL;DR</summary>

  Replaces physical repository execution with learned transition and reward models that simulate command outcomes and final test feedback.

  ![Transition][tag-transition] ![Reward][tag-reward] ![Repository][tag-repo]

  </details>

- <div><strong>CWM: An Open-Weights LLM for Research on Code Generation with World Models</strong>. arXiv 2025.<br>
  <a href="https://arxiv.org/abs/2510.02387"><img src="https://img.shields.io/badge/arXiv-Paper-B31B1B?logo=arxiv&logoColor=white" alt="arXiv"></a> <a href="https://ai.meta.com/research/publications/cwm-an-open-weights-llm-for-research-on-code-generation-with-world-models/"><img src="https://img.shields.io/badge/Website-Link-2563EB" alt="Website"></a> <a href="https://github.com/facebookresearch/cwm"><img src="https://img.shields.io/badge/Code-GitHub-2DA44E?logo=github&logoColor=white" alt="Code"></a> <a href="https://huggingface.co/facebook/cwm"><img src="https://img.shields.io/badge/Model-Hugging_Face-FFD21E" alt="Model"></a></div>
  <details>
  <summary>TL;DR</summary>

  A 32B model mid-trained on Python execution traces and agent interactions in containerized software environments.

  ![Transition][tag-transition] ![Trace][tag-trace] ![Repository][tag-repo]

  </details>

### Program Execution and State Models

- <div><strong>Towards a Neural Debugger for Python</strong>. arXiv 2026.<br>
  <a href="https://arxiv.org/abs/2603.09951"><img src="https://img.shields.io/badge/arXiv-Paper-B31B1B?logo=arxiv&logoColor=white" alt="arXiv"></a></div>
  <details>
  <summary>TL;DR</summary>

  Models forward and inverse Python execution conditioned on debugger actions such as step-into, step-over, step-return, and breakpoints.

  ![Transition][tag-transition] ![Trace][tag-trace] ![Outcome][tag-outcome]

  </details>

- <div><strong>Self-Execution Simulation Improves Coding Models</strong>. arXiv 2026.<br>
  <a href="https://arxiv.org/abs/2604.03253"><img src="https://img.shields.io/badge/arXiv-Paper-B31B1B?logo=arxiv&logoColor=white" alt="arXiv"></a></div>
  <details>
  <summary>TL;DR</summary>

  Trains step-by-step execution simulation with supervised traces and verifiable RL, then uses predicted test outcomes for self-verification and repair.

  ![Trace][tag-trace] ![Outcome][tag-outcome]

  </details>

- <div><strong>ExecVerify: White-Box RL with Verifiable Stepwise Rewards for Code Execution Reasoning</strong>. ACL 2026.<br>
  <a href="https://aclanthology.org/2026.acl-long.631/"><img src="https://img.shields.io/badge/Paper-Link-555555" alt="Paper"></a> <a href="https://github.com/tlx000000001/ExecVerify"><img src="https://img.shields.io/badge/Artifacts-GitHub-2DA44E?logo=github&logoColor=white" alt="Artifacts"></a></div>
  <details>
  <summary>TL;DR</summary>

  Rewards correct next-statement, variable-value, variable-type, and final-output predictions before transferring the capability to code generation.

  ![Trace][tag-trace] ![Outcome][tag-outcome]

  </details>

- <div><strong>What I Cannot Execute, I Do Not Understand: Training and Evaluating LLMs on Program Execution Traces</strong>. arXiv 2025.<br>
  <a href="https://arxiv.org/abs/2503.05703"><img src="https://img.shields.io/badge/arXiv-Paper-B31B1B?logo=arxiv&logoColor=white" alt="arXiv"></a></div>
  <details>
  <summary>TL;DR</summary>

  Trains line- and instruction-level execution models and studies compact and dynamic state representations for long rollouts.

  ![Trace][tag-trace] ![Outcome][tag-outcome]

  </details>

- <div><strong>SemCoder: Training Code Language Models with Comprehensive Semantics Reasoning</strong>. NeurIPS 2024.<br>
  <a href="https://proceedings.neurips.cc/paper_files/paper/2024/hash/6efcc7fd8efeee29a050a79c843c90e0-Abstract-Conference.html"><img src="https://img.shields.io/badge/Paper-Link-555555" alt="Paper"></a> <a href="https://github.com/ARiSE-Lab/SemCoder"><img src="https://img.shields.io/badge/Artifacts-GitHub-2DA44E?logo=github&logoColor=white" alt="Artifacts"></a></div>
  <details>
  <summary>TL;DR</summary>

  Learns natural-language monologues about functional behavior, local execution effects, and input/output semantics for code generation and repair.

  ![Outcome][tag-outcome]

  </details>

- <div><strong>TRACED: Execution-Aware Pre-training for Source Code</strong>. ICSE 2024.<br>
  <a href="https://arxiv.org/abs/2306.07487"><img src="https://img.shields.io/badge/arXiv-Paper-B31B1B?logo=arxiv&logoColor=white" alt="arXiv"></a> <a href="https://github.com/ARiSE-Lab/TRACED_ICSE_24"><img src="https://img.shields.io/badge/Code-GitHub-2DA44E?logo=github&logoColor=white" alt="Code"></a></div>
  <details>
  <summary>TL;DR</summary>

  Predicts execution coverage and quantized runtime variable states from C source code and inputs.

  ![Trace][tag-trace] ![Outcome][tag-outcome]

  </details>

- <div><strong>Code Execution with Pre-trained Language Models</strong>. Findings of ACL 2023.<br>
  <a href="https://aclanthology.org/2023.findings-acl.308/"><img src="https://img.shields.io/badge/Paper-Link-555555" alt="Paper"></a> <a href="https://github.com/microsoft/CodeBERT/tree/master/CodeExecutor"><img src="https://img.shields.io/badge/Code%20%26%20Data-GitHub-2DA44E?logo=github&logoColor=white" alt="Code and Data"></a></div>
  <details>
  <summary>TL;DR</summary>

  A learned Python executor that predicts line-by-line traces and final outputs, with execution pre-training transferred to code intelligence tasks.

  ![Trace][tag-trace] ![Outcome][tag-outcome]

  </details>

- <div><strong>Learning to Execute</strong>. arXiv 2014.<br>
  <a href="https://arxiv.org/abs/1410.4615"><img src="https://img.shields.io/badge/arXiv-Paper-B31B1B?logo=arxiv&logoColor=white" alt="arXiv"></a> <a href="https://github.com/wojciechz/learning_to_execute"><img src="https://img.shields.io/badge/Code-GitHub-2DA44E?logo=github&logoColor=white" alt="Code"></a></div>
  <details>
  <summary>TL;DR</summary>

  A foundational study training recurrent networks to predict program outputs directly from source code and inputs.

  ![Outcome][tag-outcome]

  </details>

### Software-World Evaluation and Analysis

- <div><strong>Towards Evaluation of Implicit Software World Models in Coding LLMs</strong>. DL4Code at ICML 2026.<br>
  <a href="https://arxiv.org/abs/2606.27406"><img src="https://img.shields.io/badge/arXiv-Paper-B31B1B?logo=arxiv&logoColor=white" alt="arXiv"></a> <a href="https://github.com/JetBrains-Research/cwm-execution-tracer"><img src="https://img.shields.io/badge/Code-GitHub-2DA44E?logo=github&logoColor=white" alt="Code"></a> <a href="https://huggingface.co/datasets/JetBrains-Research/cwm-benchmarks-dl4c-benchmark"><img src="https://img.shields.io/badge/Data-Dataset-F59E0B" alt="Data"></a></div>
  <details>
  <summary>TL;DR</summary>

  Evaluates test outcomes, exceptions, memory, runtime, and profiler predictions on library-level cases derived from SWE-bench Verified.

  </details>

- <div><strong>Debugging Code World Models</strong>. 2026.<br>
  <a href="https://arxiv.org/abs/2602.07672"><img src="https://img.shields.io/badge/arXiv-Paper-B31B1B?logo=arxiv&logoColor=white" alt="arXiv"></a> <a href="https://babak70.github.io/code-world-models-blog/"><img src="https://img.shields.io/badge/Blog-Read-8B5CF6" alt="Blog"></a></div>
  <details>
  <summary>TL;DR</summary>

  Separates action-generation failures from state-propagation failures in long execution rollouts and analyzes the effect of dense state supervision.

  </details>

### Software-World Benchmarks

Benchmarks are listed here only when they directly test learned execution or software-environment prediction; they are not themselves Code World Models.

- **2026**
  - **AgentWorldBench** — Next-observation prediction across agent environments, including dedicated Terminal and SWE subsets. [![Data][res-data]](https://huggingface.co/datasets/Qwen/AgentWorldBench)
  - **DexBench** — Paired forward-execution and counterfactual input-mutation tasks. [![Paper][res-paper]](https://aclanthology.org/2026.acl-long.735/)
  - **CoRE** — Fine-grained intermediate-state and implementation-invariance evaluation. [![Paper][res-paper]](https://aclanthology.org/2026.findings-acl.460/) [![Code][res-code]](https://github.com/ZJUSig/CoRE)
  - **CES** — Evaluates the coherence and cross-input consistency of code-execution reasoning. [![arXiv][res-arxiv]](https://arxiv.org/abs/2510.15079) [![Code][res-code]](https://github.com/Intelligent-CAT-Lab/CES)
  - **PLSemanticsBench** — Tests final states, traces, and semantic rules under both standard and deliberately altered programming-language semantics. [![arXiv][res-arxiv]](https://arxiv.org/abs/2510.03415) [![Code][res-code]](https://github.com/EngineeringSoftware/PLSemanticsBench)

- **2025**
  - **SURGE** — 1,160 problems spanning multi-language, repository-level, scientific, environment-dependent, and high-cost surrogate execution. [![Paper][res-paper]](https://aclanthology.org/2025.emnlp-main.162/) [![Code and Data][res-code-data]](https://github.com/Imbernoulli/SURGE)
  - **R-Eval** — Evaluates coverage, intermediate state and type, next statement, output, and execution consistency. [![arXiv][res-arxiv]](https://arxiv.org/abs/2403.16437) [![Website][res-website]](https://r-eval.github.io/)
  - **ThrowBench** — Runtime-exception prediction over 2,466 programs in four languages. [![arXiv][res-arxiv]](https://arxiv.org/abs/2503.04241) [![Code and Data][res-code-data]](https://github.com/giganticode/throwbench)
  - **BigO(Bench)** — Runtime and memory-complexity prediction over programming problems and solutions. [![arXiv][res-arxiv]](https://arxiv.org/abs/2503.15242) [![Code and Data][res-code-data]](https://github.com/facebookresearch/BigOBench)

- **2024**
  - **CRUXEval** — Input and output prediction for short Python functions. [![Paper][res-paper]](https://proceedings.mlr.press/v235/gu24c.html) [![Code and Data][res-code-data]](https://github.com/facebookresearch/cruxeval)
  - **Code Simulation Challenges** — Tests straight-line execution, critical paths, redundant computation, loops, and sorting. [![arXiv][res-arxiv]](https://arxiv.org/abs/2401.09074) [![Code and Data][res-code-data]](https://github.com/EmanueleLM/CodeSimulation)

## Benchmark Selection Guide

Choose a benchmark by the capability being tested. The capability groups below are a reading aid for the benchmarks already collected in this repository; no single benchmark covers all aspects of a world model.

### External Worlds

| Capability | Benchmark | Task / evidence to inspect |
| --- | --- | --- |
| Persistent visual state | [CombatStateBench](https://arxiv.org/abs/2609.10540) | Entity counts and state accuracy across camera motion, occlusion, and irreversible events. |
| Physical reconstruction | [VisPhyBench](https://arxiv.org/abs/2602.13294) | Generate simulator code from visual evidence; assess reconstructed appearance and motion. |
| Executable dynamics for planning | [CWMB](https://arxiv.org/abs/2405.15383) | Synthesize simulators from descriptions and trajectories; evaluate model fidelity and planning. |
| Symbolic rule generation | [Text2World](https://aclanthology.org/2025.findings-acl.1337/) | Generate PDDL world models from text; assess them through execution-based criteria. |

### Software Environments

| Capability | Benchmark(s) | Task / evidence to inspect |
| --- | --- | --- |
| Tool and environment transitions | [AgentWorldBench](https://huggingface.co/datasets/Qwen/AgentWorldBench), Terminal / SWE subsets | Predict observations after shell commands or repository actions against real execution feedback. |
| Input / output reasoning | [CRUXEval](https://proceedings.mlr.press/v235/gu24c.html), [DexBench](https://aclanthology.org/2026.acl-long.735/) | Infer function inputs or outputs; DexBench additionally tests input mutations toward behavioral goals. |
| Intermediate execution behavior | [R-Eval](https://arxiv.org/abs/2403.16437), [CoRE](https://aclanthology.org/2026.findings-acl.460/) | Inspect execution paths and intermediate states; test consistency beyond final outputs. |
| Reasoning under controlled changes | [CES](https://arxiv.org/abs/2510.15079), [PLSemanticsBench](https://arxiv.org/abs/2510.03415) | CES tests trace coherence across inputs; PLSemanticsBench tests adherence to supplied and altered semantics. |
| Broader execution settings | [SURGE](https://aclanthology.org/2025.emnlp-main.162/), [Code Simulation Challenges](https://arxiv.org/abs/2401.09074) | SURGE spans languages and execution environments; Code Simulation Challenges varies computational structures. |
| Exceptions and resource behavior | [ThrowBench](https://arxiv.org/abs/2503.04241), [BigO(Bench)](https://arxiv.org/abs/2503.15242) | Predict runtime exceptions or time and space complexity, respectively. |

**When comparing results**, check the available inputs, access to a real environment or executor, rollout horizon, evaluation split, and model-construction budget. For visual models, inspect state correctness and rendered appearance separately. For planning models, examine model fidelity alongside downstream success. For software models, distinguish final-output accuracy from faithful intermediate execution.

## Reading Paths

These suggested paths connect complementary design choices; arrows indicate reading order, not a claim of direct technical lineage.

- **Persistent visual worlds:** [PWM](https://arxiv.org/abs/2609.10540) → [Coding Agent as World Brain](https://arxiv.org/abs/2608.25927) → [Magpie](https://arxiv.org/abs/2608.27168). Compare agent-written dynamics with a conventional engine driving a neural renderer.
- **Learning simulators for planning:** [WorldCoder](https://proceedings.neurips.cc/paper_files/paper/2024/hash/820c61a0cd419163ccbd2c33b268816e-Abstract-Conference.html) → [GIF-MCTS](https://arxiv.org/abs/2405.15383) → [PoE-World](https://proceedings.neurips.cc/paper_files/paper/2025/hash/262dd62fd1bbb30d6a6b4d578f5e65ff-Abstract-Conference.html). Compare interaction-driven induction, program search, and compositional stochastic dynamics.
- **Inferring executable physics:** [VisPhyWorld](https://arxiv.org/abs/2602.13294) → [Code as Worlds](https://arxiv.org/abs/2608.27549). Examine how visual evidence constrains simulator code and how execution makes hypotheses testable.
- **Software execution as the modeled world:** [CodeExecutor](https://aclanthology.org/2023.findings-acl.308/) → [Meta CWM](https://arxiv.org/abs/2510.02387) → [SWE-World](https://arxiv.org/abs/2602.03419). Move from program traces to repository interaction and learned environment feedback.

## Research Questions

The following questions are our synthesis of the design space, intended to guide discussion and future additions rather than to assert that every listed method has the same limitations.

- **Learning versus specifying rules:** How much of a world's dynamics comes from observed evidence, and how much is supplied in prompts, libraries, or a simulator? Can the model identify rules when these sources are incomplete or conflict?
- **Partial observability and uncertainty:** How should executable models represent hidden state, multiple plausible explanations, and stochastic events while remaining useful for planning?
- **State-to-image consistency:** How can we test that a neural renderer obeys the executable state through occlusion, camera changes, and long interactions? Can visual feedback reveal and repair mismatches?
- **Continual program revision:** When new evidence requires a code change, how can the model preserve accumulated state and previously correct behavior? What regression checks should accompany a repair?
- **Compositional generalization:** Can learned entities and rules be reused in unfamiliar combinations, rather than only replaying the scenes or trajectories used to construct the model?
- **Quality and cost together:** What is the full cost of constructing, verifying, updating, and rolling out a model? How do planning quality and fidelity change under matched interaction and compute budgets?

## Terminology and Scope

“Code World Model” currently refers to several related but distinct ideas:

1. **Programmatic external worlds** — code represents another world's entities, state, and dynamics. This is the repository's primary meaning.
2. **Code-state visual worlds** — executable state is compiled into controls for a neural image or video renderer. PWM and *Coding Agent as World Brain* are the clearest examples.
3. **Software world models** — a learned model predicts how source code, commands, repositories, or tests execute. Meta CWM and SWE-World use this meaning.

The boundary is representation, not appearance: a world may be visually simple and still qualify if its dynamics are executable programs. Conversely, a photorealistic interactive video model is adjacent rather than core if all persistent state remains implicit in pixels or latent features.

## Contributing

Contributions are welcome. Before opening a pull request, please check that the resource:

1. Uses executable code or another programmatic representation as a world model, couples explicit state to a generative renderer, **or** clearly belongs to the separately labeled software-world direction.
2. Does more than generate code: the program must represent, advance, reconstruct, evaluate, or support planning in a world.
3. Links to a canonical paper/project page and, when available, public code, data, and model weights.
4. Includes one sentence explaining the represented world, how state evolves or is predicted, and how observations or feedback are produced.
5. Updates the matching comparison table for a model entry, or the benchmark selection guide for a benchmark entry. Keep table claims grounded in the linked primary source; use “Not reported” when a relevant detail is unavailable.

Please keep entries reverse chronological within each section and prefer archival conference pages over secondary summaries.

## Acknowledgements

The organization of this repository is inspired by [Awesome World Models](https://github.com/knightnemo/Awesome-World-Models) and [Awesome WAM](https://github.com/OpenMOSS/Awesome-WAM). Thanks to their maintainers and to everyone building open resources for world-model research.

If you find a missing paper or a classification mistake, please open an issue or submit a pull request.

[tag-code-state]: https://img.shields.io/badge/code_state-8B7BD8?style=flat-square
[tag-agent-written]: https://img.shields.io/badge/agent_written-A78BDB?style=flat-square
[tag-induced]: https://img.shields.io/badge/induced-E7B75F?style=flat-square
[tag-neural-renderer]: https://img.shields.io/badge/neural_renderer-63B995?style=flat-square
[tag-explicit-state]: https://img.shields.io/badge/explicit_state-6E9FDB?style=flat-square
[tag-software-world]: https://img.shields.io/badge/software_world-94A3B8?style=flat-square
[tag-transition]: https://img.shields.io/badge/transition-7CB8D4?style=flat-square
[tag-trace]: https://img.shields.io/badge/trace-A78BDB?style=flat-square
[tag-outcome]: https://img.shields.io/badge/outcome-E8A87C?style=flat-square
[tag-reward]: https://img.shields.io/badge/reward-8FBC8F?style=flat-square
[tag-repo]: https://img.shields.io/badge/repository-94A3B8?style=flat-square
[tag-terminal]: https://img.shields.io/badge/terminal-6B7280?style=flat-square

[res-arxiv]: https://img.shields.io/badge/arXiv-Paper-B31B1B?logo=arxiv&logoColor=white
[res-paper]: https://img.shields.io/badge/Paper-Link-555555
[res-website]: https://img.shields.io/badge/Website-Link-2563EB
[res-code]: https://img.shields.io/badge/Code-GitHub-2DA44E?logo=github&logoColor=white
[res-data]: https://img.shields.io/badge/Data-Dataset-F59E0B
[res-code-data]: https://img.shields.io/badge/Code%20%26%20Data-GitHub-2DA44E?logo=github&logoColor=white
