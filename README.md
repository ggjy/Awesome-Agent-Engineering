# Awesome Agent Engineering

> A curated collection of papers, systems, benchmarks, and engineering resources for **LLM-based agent systems**, organized through a **model-harness lens**.

This repository accompanies the survey:

**From Question Answering to Task Completion: A Survey on Agent System and Harness Design**

LLM-based agents are moving from passive question answering to active task completion. They perceive environments, invoke tools, maintain state, recover from errors, and act over extended horizons. This repository organizes the literature around the engineering shift from **prompt engineering** to **context/workflow engineering**, **harness engineering**, and **agent-native training**.

If you find this repository useful, please consider starring it and citing our survey.

---

## News

- **[2026.06]** Repository initialized for the survey *From Question Answering to Task Completion: A Survey on Agent System and Harness Design*.
- **[2026.06]** Initial taxonomy and paper list released.

---

## Table of Contents

- [Overview](#overview)
- [Taxonomy](#taxonomy)
- [Paper List](#paper-list)
  - [Surveys and Foundations](#surveys-and-foundations)
  - [Prompting, Reasoning, and Planning](#prompting-reasoning-and-planning)
  - [Workflow, Context, and Tool Use](#workflow-context-and-tool-use)
  - [Harness, Runtime, Memory, and Protocols](#harness-runtime-memory-and-protocols)
  - [Benchmarks, Evaluation, and Safety](#benchmarks-evaluation-and-safety)
  - [Domain Agents](#domain-agents)
  - [Agent-Native Training and Self-Evolution](#agent-native-training-and-self-evolution)
- [Representative Systems](#representative-systems)
- [Benchmarks](#benchmarks)
- [Contributing](#contributing)
- [Citation](#citation)

---

## Overview

The central question of this survey is:

> When agentic tasks remain unreliable, is the bottleneck primarily the foundation model, or the runtime system surrounding it?

We argue that agent quality emerges from the interaction between:

- **Model capability**: reasoning, perception, planning, instruction following.
- **Runtime infrastructure**: tools, context, memory, state, sandboxing, verification, recovery.
- **Task structure**: horizon, environment, observability, oracle strength, risk.
- **Evaluation design**: success criteria, cost, latency, safety, trace quality.

Instead of treating agents as models with auxiliary tools, we view an LLM-based agent as:

```text
Agent = Foundation Model + Execution Harness
```

The execution harness is the runtime system that determines what the model observes, which actions it can take, how state persists, how failures are detected, and how outcomes are verified.

---

## Taxonomy

### Four Paradigms of Agent Engineering

| Paradigm | Core Question | Main Lever | Typical Limitation |
| --- | --- | --- | --- |
| Prompt Engineering | How should we ask the model? | Instructions, exemplars, reasoning prompts | Does not manage state, tools, or recovery |
| Workflow & Context Engineering | What information should enter the model context? | Retrieval, memory, tool definitions, context compression | Mostly feedforward; weak recovery and verification |
| Harness Engineering | How do we keep the whole system on track? | Runtime loop, sandbox, state, tools, verifiers, rollback | Requires task-specific engineering and evaluation |
| Agent-Native Training | What agentic behavior should be internalized into the model? | RL, trajectory learning, tool-use training, self-evolution | Still requires harnesses for data, verification, and governance |

### Six Runtime Responsibilities

| Component | Role in Agent Systems |
| --- | --- |
| Observation Interface | Converts environment state into model-usable observations |
| Context Manager | Selects, compresses, retrieves, and formats information |
| Control Loop | Decides when to plan, act, verify, retry, stop, or escalate |
| Action Interface | Exposes tools, APIs, browser/GUI controls, shell commands, or robot actions |
| State and Artifact Store | Persists memory, logs, files, checkpoints, traces, and intermediate outputs |
| Verification and Governance | Checks correctness, safety, permissions, policy compliance, and recovery |

### Task-to-Harness Pressure Mapping

| Task Property | Failure Pressure | Harness Response |
| --- | --- | --- |
| Long horizon | State drift and context loss | Checkpoints, summaries, artifacts, memory |
| Partial observability | Hidden or indirect state | Structured observations and grounding |
| Strong oracle | Checkable outcomes | Verifier loops and repair cycles |
| Weak or delayed oracle | Uncertain success | Provenance, review, approval, conservative stopping |
| Irreversible actions | Persistent side effects | Sandbox, gates, rollback, permissions |
| High autonomy / low latency | Limited human correction | Budgets, controllers, logging, escalation |

---

## Paper List

### Surveys and Foundations

- **Language Models are Few-Shot Learners**. Brown et al. NeurIPS 2020. [[paper](https://arxiv.org/abs/2005.14165)]
- **Training Language Models to Follow Instructions with Human Feedback**. Ouyang et al. NeurIPS 2022. [[paper](https://arxiv.org/abs/2203.02155)]
- **A Survey on Large Language Model based Autonomous Agents**. Wang et al. Frontiers of Computer Science 2024. [[paper](https://arxiv.org/abs/2308.11432)]
- **The Rise and Potential of Large Language Model Based Agents: A Survey**. Xi et al. Science China Information Sciences 2025. [[paper](https://arxiv.org/abs/2309.07864)]
- **Large Language Model Agent: A Survey on Methodology, Applications and Challenges**. Luo et al. 2025. [[paper](https://arxiv.org/abs/2503.21460)]
- **Large Language Model Based Multi-Agents: A Survey of Progress and Challenges**. Guo et al. IJCAI 2024. [[paper](https://arxiv.org/abs/2402.01680)]
- **A Survey on LLM-based Multi-Agent Systems: Workflow, Infrastructure, and Challenges**. Li et al. 2024.
- **Survey on Evaluation of LLM-based Agents**. Yehudai et al. 2025.
- **GUI Agents: A Survey**. Nguyen et al. ACL Findings 2025. [[paper](https://arxiv.org/abs/2412.13501)]
- **A Survey on Vision-Language-Action Models for Embodied AI**. Ma et al. 2024. [[paper](https://arxiv.org/abs/2405.14093)]
- **A Survey on Trustworthy LLM Agents: Threats and Countermeasures**. Yu et al. KDD 2025. [[paper](https://arxiv.org/abs/2503.09648)]

### Prompting, Reasoning, and Planning

- **Chain-of-Thought Prompting Elicits Reasoning in Large Language Models**. Wei et al. NeurIPS 2022. [[paper](https://arxiv.org/abs/2201.11903)]
- **Self-Consistency Improves Chain of Thought Reasoning in Language Models**. Wang et al. 2022. [[paper](https://arxiv.org/abs/2203.11171)]
- **Tree of Thoughts: Deliberate Problem Solving with Large Language Models**. Yao et al. NeurIPS 2023. [[paper](https://arxiv.org/abs/2305.10601)]
- **ReAct: Synergizing Reasoning and Acting in Language Models**. Yao et al. ICLR 2023. [[paper](https://arxiv.org/abs/2210.03629)]
- **Reflexion: Language Agents with Verbal Reinforcement Learning**. Shinn et al. NeurIPS 2023. [[paper](https://arxiv.org/abs/2303.11366)]
- **Self-Refine: Iterative Refinement with Self-Feedback**. Madaan et al. NeurIPS 2023. [[paper](https://arxiv.org/abs/2303.17651)]
- **Voyager: An Open-Ended Embodied Agent with Large Language Models**. Wang et al. TMLR 2023. [[paper](https://arxiv.org/abs/2305.16291)]
- **Language Agent Tree Search Unifies Reasoning, Acting, and Planning**. Zhou et al. 2023. [[paper](https://arxiv.org/abs/2310.04406)]

### Workflow, Context, and Tool Use

- **Retrieval-Augmented Generation for Knowledge-Intensive NLP Tasks**. Lewis et al. NeurIPS 2020. [[paper](https://arxiv.org/abs/2005.11401)]
- **Toolformer: Language Models Can Teach Themselves to Use Tools**. Schick et al. NeurIPS 2023. [[paper](https://arxiv.org/abs/2302.04761)]
- **Gorilla: Large Language Model Connected with Massive APIs**. Patil et al. 2023. [[paper](https://arxiv.org/abs/2305.15334)]
- **MemGPT: Towards LLMs as Operating Systems**. Packer et al. 2023. [[paper](https://arxiv.org/abs/2310.08560)]
- **AutoGen: Enabling Next-Gen LLM Applications via Multi-Agent Conversation**. Wu et al. 2023. [[paper](https://arxiv.org/abs/2308.08155)]
- **MetaGPT: Meta Programming for a Multi-Agent Collaborative Framework**. Hong et al. ICLR 2024. [[paper](https://arxiv.org/abs/2308.00352)]
- **Effective Context Engineering for AI Agents**. Anthropic Engineering 2025. [[blog](https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents)]
- **Context as a Tool: Context Management for Long-Horizon SWE-Agents**. Liu et al. 2025.
- **ContextBench: A Benchmark for Context Retrieval in Coding Agents**. Li et al. 2026.
- **Agentic Context Engineering: Evolving Contexts for Self-Improving Language Models**. Zhang et al. 2025.

### Harness, Runtime, Memory, and Protocols

- **SWE-agent: Agent-Computer Interfaces Enable Automated Software Engineering**. Yang et al. NeurIPS 2024. [[paper](https://arxiv.org/abs/2405.15793)]
- **OpenHands: An Open Platform for AI Software Developers as Generalist Agents**. Wang et al. 2024. [[paper](https://arxiv.org/abs/2407.16741)]
- **Building Effective Agents**. Anthropic Engineering 2024. [[blog](https://www.anthropic.com/engineering/building-effective-agents)]
- **Effective Harnesses for Long-Running Agents**. Anthropic Engineering 2025. [[blog](https://www.anthropic.com/engineering/effective-harnesses-for-long-running-agents)]
- **Harness Engineering: Leveraging Codex in an Agent-First World**. OpenAI 2026. [[blog](https://openai.com/index/harness-engineering/)]
- **Model Context Protocol**. Anthropic 2025. [[website](https://modelcontextprotocol.io/introduction)]
- **Agent2Agent Protocol**. Google Cloud 2025. [[github](https://github.com/a2aproject/A2A)]
- **OpenAI Agents SDK**. OpenAI 2025. [[github](https://github.com/openai/openai-agents-python)]
- **Natural-Language Agent Harnesses**. Pan et al. 2026.
- **Meta-Harness: End-to-End Optimization of Model Harnesses**. Lee et al. 2026.
- **OpenSquilla: Token-Efficient AI Agent with Same Budget, Higher Intelligence Density**. OpenSquilla Team 2026. [[github](https://github.com/opensquilla/opensquilla)]
- **Structurally Aligned Subtask-Level Memory for Software Engineering Agents**. Shen et al. 2026.
- **MemMA: Coordinating the Memory Cycle through Multi-Agent Reasoning and In-Situ Self-Evolution**. Lin et al. 2026.

### Benchmarks, Evaluation, and Safety

- **SWE-bench: Can Language Models Resolve Real-World GitHub Issues?** Jimenez et al. ICLR 2024. [[paper](https://arxiv.org/abs/2310.06770)] [[leaderboard](https://www.swebench.com/)]
- **WebArena: A Realistic Web Environment for Building Autonomous Agents**. Zhou et al. ICLR 2024. [[paper](https://arxiv.org/abs/2307.13854)]
- **VisualWebArena: Evaluating Multimodal Agents on Realistic Visual Web Tasks**. Koh et al. ACL 2024. [[paper](https://arxiv.org/abs/2401.13649)]
- **OSWorld: Benchmarking Multimodal Agents for Open-Ended Tasks in Real Computer Environments**. Xie et al. NeurIPS 2024. [[paper](https://arxiv.org/abs/2404.07972)]
- **TheAgentCompany: Benchmarking LLM Agents on Consequential Real World Tasks**. Xu et al. 2024. [[paper](https://arxiv.org/abs/2412.14161)]
- **Terminal-Bench: Benchmarking Agents on Hard, Realistic Tasks in Command Line Interfaces**. Merrill et al. 2026. [[leaderboard](https://www.tbench.ai/leaderboard/terminal-bench/2.0)]
- **AgentBench: Evaluating LLMs as Agents**. Liu et al. 2023. [[paper](https://arxiv.org/abs/2308.03688)]
- **GAIA: A Benchmark for General AI Assistants**. Mialon et al. ICLR 2024. [[paper](https://arxiv.org/abs/2311.12983)]
- **MLAgentBench: Evaluating Language Agents on Machine Learning Experimentation**. Huang et al. 2023. [[paper](https://arxiv.org/abs/2310.03302)]
- **MMAU: A Holistic Benchmark of Agent Capabilities Across Diverse Domains**. Yin et al. 2024.
- **OS-Harm: A Benchmark for Measuring Safety of Computer Use Agents**. Kuntz et al. 2025.
- **ReliabilityBench: Evaluating LLM Agent Reliability under Production-like Stress Conditions**. Gupta 2026.
- **Beyond Task Completion: Revealing Corrupt Success in LLM Agents through Procedure-Aware Evaluation**. Cao et al. 2026.
- **CostBench: Evaluating Multi-Turn Cost-Optimal Planning and Adaptation in Dynamic Environments for LLM Tool-Use Agents**. Liu et al. 2025.
- **Holistic Agent Leaderboard: The Missing Infrastructure for AI Agent Evaluation**. Kapoor et al. 2025.

### Domain Agents

#### Software Engineering Agents

- **Devin**. Cognition Labs 2024. [[blog](https://www.cognition.ai/blog/introducing-devin)]
- **Claude Code**. Anthropic. [[docs](https://docs.claude.com/en/docs/claude-code/how-claude-code-works)]
- **Codex**. OpenAI. [[product](https://openai.com/codex/)]
- **Agentless: Demystifying LLM-based Software Engineering Agents**. Xia et al. FSE 2025.
- **PatchPilot: A Stable and Cost-Efficient Agentic Patching Framework**. Li et al. 2025.
- **Repo2Run: Automated Building Executable Environment for Code Repository at Scale**. Hu et al. NeurIPS 2026.
- **R2E-Gym: Procedural Environments and Hybrid Verifiers for Scaling Open-Weights SWE Agents**. Jain et al. 2025.
- **Wink: Recovering from Misbehaviors in Coding Agents**. Nanda et al. 2026.

#### Web, GUI, and Computer-Use Agents

- **AppAgent: Multimodal Agents as Smartphone Users**. Zhang et al. CHI 2025. [[paper](https://arxiv.org/abs/2312.13771)]
- **Mobile-Agent: Autonomous Multi-Modal Mobile Device Agent with Visual Perception**. Wang et al. 2024. [[paper](https://arxiv.org/abs/2401.16158)]
- **You Only Look at Screens: Multimodal Android Agents with Visual Perception**. Zhan and Zhang 2024.
- **MCPWorld: A Unified Benchmarking Testbed for API, GUI, and Hybrid Computer Use Agents**. Yan et al. 2025.
- **OSWorld-MCP: Benchmarking MCP Tool Invocation in Computer-Use Agents**. Jia et al. 2025.
- **Agent S2: A Compositional Generalist-Specialist Framework for Computer Use Agents**. Agashe et al. 2025.
- **ToolTok: Tool Tokenization for Efficient and Generalizable GUI Agents**. Wang et al. 2026.
- **Surfer 2: The Next Generation of Cross-Platform Computer Use Agents**. Andreux et al. 2025.
- **FARA-7B: An Efficient Agentic Model for Computer Use**. Awadallah et al. 2025.

#### Scientific and Research Agents

- **ChemCrow: Augmenting Large-Language Models with Chemistry Tools**. Bran et al. 2023. [[paper](https://arxiv.org/abs/2304.05376)]
- **Autonomous Chemical Research with Large Language Models**. Boiko et al. Nature 2023. [[paper](https://www.nature.com/articles/s41586-023-06792-0)]
- **The AI Scientist: Towards Fully Automated Open-Ended Scientific Discovery**. Lu et al. 2024. [[paper](https://arxiv.org/abs/2408.06292)]
- **AI Scientist-v2: Workshop-Level Automated Scientific Discovery via Agentic Tree Search**. Yamada et al. 2025.
- **Agent Laboratory: Using LLM Agents as Research Assistants**. Schmidgall et al. 2025.
- **Towards an AI Co-Scientist**. Gottweis et al. 2025.
- **Kosmos: An AI Scientist for Autonomous Discovery**. Mitchener et al. 2025.
- **AlphaEvolve: A Coding Agent for Scientific and Algorithmic Discovery**. Novikov et al. 2025.
- **Sibyl-AutoResearch: Autonomous Research Needs Self-Evolving Trial-and-Error Harnesses, Not Paper Generators**. Wang et al. 2026.

#### Embodied and Robotics Agents

- **AndroidEnv: A Reinforcement Learning Platform for Android**. Toyama et al. 2021. [[paper](https://arxiv.org/abs/2105.13231)]
- **Embodied Cooperation among LLM Agents**. Yang et al. 2026.
- **Explore with Long-Term Memory: A Benchmark and MLLM-based RL Framework for Embodied Exploration**. Wang et al. 2026.
- **Cap-X: Benchmarking and Improving Coding Agents for Robot Manipulation**. Fu et al. 2026.
- **When Should a Robot Think? Resource-Aware Reasoning via Reinforcement Learning for Embodied Robotic Decision-Making**. Liu et al. 2026.
- **Robosafe: Safeguarding Embodied Agents via Executable Safety Logic**. Wang et al. 2025.

### Agent-Native Training and Self-Evolution

- **DeepSeek-R1: Incentivizing Reasoning Capability in LLMs via Reinforcement Learning**. Guo et al. 2025. [[paper](https://arxiv.org/abs/2501.12948)]
- **DAPO: An Open-Source LLM Reinforcement Learning System at Scale**. Yu et al. 2025.
- **WebRL: Training LLM Web Agents via Self-Evolving Online Curriculum Reinforcement Learning**. Qi et al. 2024.
- **ComputerRL: Scaling End-to-End Online Reinforcement Learning for Computer Use Agents**. Lai et al. 2025.
- **Do Not Just Fine-Tune the Agent, Tune the Environment**. Lu et al. 2025.
- **Davinci-Dev: Agent-Native Mid-Training for Software Engineering**. Zeng et al. 2026.
- **Kimi-Dev: Agentless Training as Skill Prior for SWE-Agents**. Yang et al. 2025.
- **Evolver: Self-Evolving LLM Agents through an Experience-Driven Lifecycle**. Wu et al. 2025.
- **AgentEvolver: Towards Efficient Self-Evolving Agent System**. Zhai et al. 2025.
- **Continual Harness: Online Adaptation for Self-Improving Foundation Agents**. Karten et al. 2026.
- **Darwin Godel Machine: Open-Ended Evolution of Self-Improving Agents**. Zhang et al. 2025.
- **Chain-of-Agents: End-to-End Agent Foundation Models via Multi-Agent Distillation and Agentic RL**. Li et al. 2025.
- **WebAgent-R1: Training Web Agents via End-to-End Multi-Turn Reinforcement Learning**. Wei et al. 2025.
- **Agent Q-Mix: Selecting the Right Action for LLM Multi-Agent Systems through Reinforcement Learning**. Jiang et al. 2026.

---

## Representative Systems

| System | Domain | Harness Style | Key Design |
| --- | --- | --- | --- |
| SWE-agent | Software engineering | ReAct-style loop | Shell/editor interface, iterative inspect-edit-test |
| Agentless | Software engineering | Fixed pipeline | Localization, repair generation, patch selection |
| AutoCodeRover | Software engineering | Search-guided repair | Repository-aware code search and validation |
| OpenHands | Software engineering | General runtime agent | Shell, editor, browser/tools, iterative execution |
| PatchPilot | Software engineering | Structured repair workflow | Reproduction, localization, validation, refinement |
| Codex | Software engineering | Managed coding agent | Code execution, editing, task management |
| AppAgent | Mobile GUI | Exploration-driven | Smartphone interaction as agent task |
| Mobile-Agent | Mobile GUI | Vision-grounded loop | OCR, icon detection, visual perception |
| OpenSquilla | General agents | Token-efficient runtime | Higher intelligence density under fixed budget |
| Sibyl-AutoResearch | Research agents | Self-evolving harness | Trial-and-error memory, evidence gates, repair loops |

---

## Benchmarks

| Benchmark | Focus | Environment | Primary Signal |
| --- | --- | --- | --- |
| SWE-bench | Coding | Real GitHub issues | Resolution rate |
| WebArena | Web | Realistic websites | Task success |
| VisualWebArena | Multimodal web | Visual web tasks | Task success |
| OSWorld | Desktop | Real OS | Multi-app success |
| Terminal-Bench | Terminal / coding | Command line | Task success |
| TheAgentCompany | Enterprise tasks | Simulated company | Task success |
| AgentBench | General agents | Interactive environments | Task completion |
| GAIA | General assistants | Multi-step tasks | Accuracy |
| MLAgentBench | ML engineering | Experimentation tasks | Performance improvement |
| OS-Harm | Safety | Desktop computer use | Harmful action rate |
| LoCoMo | Long-term memory | Multi-session dialogue | QA / consistency |
| MCPWorld | API + GUI | Hybrid tool environments | Task success / tool use |
| MCPAgentBench | MCP tool use | Real MCP servers | Tool-use success |

---

## Value-Aware Evaluation

Agent evaluation should not only report task success. Practical deployment also depends on:

- Cost: tokens, API calls, tool calls, compute, infrastructure.
- Latency: wall-clock time, step count, P95/P99 tail latency.
- Reliability: repeated-run consistency, stress robustness, recovery.
- Safety: policy violations, permission boundaries, side effects.
- Process quality: trace inspectability, provenance, verifier use, rollback.

Two systems with the same success rate may differ dramatically in cost, risk, auditability, and deployment value.

---

## Contributing

Contributions are welcome. You can help by:

- Adding missing papers, systems, or benchmarks.
- Fixing broken links or metadata.
- Suggesting better taxonomy categories.
- Adding concise summaries for important papers.
- Opening issues for correction, clarification, or discussion.

Suggested entry format:

```markdown
- **Paper Title**. Author et al. Venue/Year. [[paper](URL)] [[code](URL)]  
  Short note about why it is relevant to agent engineering.
```

---

## Citation

If this repository or the survey is useful for your work, please cite:

```bibtex
@article{guo2026agentengineering,
  title   = {From Question Answering to Task Completion: A Survey on Agent System and Harness Design},
  author  = {Guo, Jianyuan and Hao, Zhiwei and Wang, Chengcheng and Fan, Cheng and Luo, Tingzhang and Li, Hongguang and Gao, Ying and Mei, Hefei and Peng, Jiankun and Xu, Rongjian and Dong, Minjing and Wu, Han and Zheng, Mengyu and Zhu, Mingjian and Han, Kai and Xu, Chang and Wang, Shiqi and Wang, Yunhe},
  year    = {2026},
  journal = {arXiv preprint},
}
```

---

## License

This repository is released under the MIT License unless otherwise specified. Paper copyrights belong to their respective authors.

