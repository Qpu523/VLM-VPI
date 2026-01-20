# LLM-Driver: A Vision–Language–Enhanced Multimodal Reasoning Framework for Vehicle Simulation

[cite_start]This repository hosts the implementation and evaluation of **LLM-Driver**, a closed-loop autonomous driving framework that integrates **multimodal large language models (MLLMs)** into the CARLA simulator[cite: 30, 43]. [cite_start]It addresses the fundamental challenge of pedestrian intent prediction and safety-critical control in urban environments by bridging the gap between geometric perception and semantic reasoning[cite: 34].

---

## 🧠 Framework Overview

[cite_start]The **LLM-Driver framework** shifts autonomous vehicle decision-making from purely physics-based prediction to semantic reasoning in social space[cite: 14]. The system features:

-   [cite_start]**Multimodal Reasoning Layer:** Combines **Vision-Language Models (Qwen3-VL)** for visual scene description with **Reasoning Models (GPT-o1)** for intent inference[cite: 53, 54].
-   [cite_start]**Few-Shot Learning:** Leverages real-world behavioral exemplars (extracted from the PIE dataset) to ground reasoning in authentic human interactions[cite: 89].
-   [cite_start]**Demographic-Adaptive Control:** A tiered safety controller that adjusts braking margins based on pedestrian age groups (Child, Adult, Senior) to ensure equitable safety protection[cite: 131].

<p align="center">
  <img src="images/Figure1.png" alt="Framework Architecture" width="800">
  <br>
  [cite_start]<em>Figure 1: The LLM-Driver framework architecture operating in closed-loop mode within CARLA[cite: 45].</em>
</p>

---

## ⚙️ Methodology

[cite_start]The system operates via an asynchronous two-level architecture[cite: 44]:

### 1. Upper Level: Multimodal Perception and Reasoning
[cite_start]This module infers latent pedestrian crossing intentions (Yielding vs. Non-Yielding) using a chain-of-thought process[cite: 69].

* **Multimodal Inputs:**
    * [cite_start]**Vision:** Front-view camera image processed by Qwen3-VL 8B to generate structured scene descriptions[cite: 53].
    * [cite_start]**Kinematics:** Historical trajectory logs (JSON format) containing vehicle and pedestrian states[cite: 81].
* **Reasoning Core:**
    * [cite_start]The reasoning model (GPT-o1 20B) receives the scene description, kinematic logs, and **few-shot real-world exemplars**[cite: 54].
    * [cite_start]It outputs a structured analysis including Visual Analysis, Kinematic Analysis, Decision, and Reasoning[cite: 106].

<p align="center">
  <img src="images/Figure2.png" alt="Real-world Exemplar" width="800">
  <br>
  [cite_start]<em>Figure 2: A real-world few-shot exemplar used in the prompt, integrating visual context, kinematic logs, and structured reasoning[cite: 96].</em>
</p>

### 2. Lower Level: Demographic-Adaptive Tiered Safety Control
[cite_start]Translates high-level intent into precise control commands[cite: 123].

* **Control Logic:**
    * [cite_start]**Yielding:** Vehicle maintains Autopilot mode[cite: 128].
    * [cite_start]**Non-Yielding:** Vehicle switches to Emergency mode[cite: 129].
* [cite_start]**Demographic Adaptation ($\alpha_{demo}$):** Safety margins are scaled based on detected demographic groups to account for varying risk profiles[cite: 137].
    * [cite_start]**Child ($\alpha = 1.4$):** High behavioral uncertainty[cite: 133].
    * [cite_start]**Senior ($\alpha = 1.2$):** Reduced mobility[cite: 134].
    * [cite_start]**Adult ($\alpha = 1.0$):** Baseline behavior[cite: 135].
* [cite_start]**Braking Tiers:** Implements graduated deceleration ($0.2g, 0.4g, 0.7g, 1.0g$) based on collision imminence[cite: 156].

<p align="center">
  <img src="images/Figure3.png" alt="Control Architecture" width="800">
  <br>
  [cite_start]<em>Figure 3: Demographic-Adaptive Tiered Safety Control Architecture[cite: 139].</em>
</p>

---

## 📊 Results

[cite_start]Experiments were conducted in **CARLA Town10HD** using 112 intent classification scenarios and 200 safety evaluation scenarios[cite: 194, 271].

### Classification Performance (Ablation Study)
[cite_start]The integration of real-world few-shot priors provides increasing marginal benefits as scenario complexity rises[cite: 218].

| Method | Accuracy | Recall (Non-Yield) | False Negative Rate |
| :--- | :--- | :--- | :--- |
| Rule-based Baseline | 78.4% | 81.3% | 18.7% |
| Vision-only LLM | 82.8% | 84.0% | 16.0% |
| Kinematics-only LLM | 83.9% | 85.3% | 14.7% |
| Zero-shot LLM | 88.4% | 89.8% | 10.2% |
| **Real-world Few-shot LLM** | **92.3%** | **94.1%** | **5.9%** |

[cite_start]*[Data Source: Table 3 [cite: 208]]*

### Safety Analysis
The LLM-Driver significantly shifts the Time-to-Collision (TTC) distribution toward safer values.
* [cite_start]**Conflict Reduction:** Reduces extreme-risk interactions (TTC < 2.0s) by **73%** compared to the rule-based baseline[cite: 289].
* [cite_start]**Traffic Efficiency:** Achieves a **62% reduction** in unnecessary braking events in yielding scenarios[cite: 322].

<p align="center">
  <img src="images/Figure4.png" alt="TTC Distribution" width="800">
  <br>
  [cite_start]<em>Figure 4: Distribution of minimum TTC values across system configurations[cite: 280].</em>
</p>

### Control Dynamics
Unlike reactive baselines where TTC continuously degrades, LLM-Driver stabilizes TTC within the trigger zone through anticipatory intent recognition.

<p align="center">
  <img src="images/Figure5.png" alt="TTC Evolution" width="800">
  <br>
  [cite_start]<em>Figure 5: Temporal TTC evolution comparing (a) Rule-based baseline and (b) LLM-Driver with few-shot reasoning[cite: 298].</em>
</p>

---

## 🔬 Qualitative Demonstration

The system generates interpretable reasoning chains explaining *why* a classification was made. Below are representative cases showing how the model distinguishes intent across demographics.

<p align="center">
  <img src="images/Figure6.png" alt="Reasoning Demonstration" width="800">
  <br>
  <em>Figure 6: Vision–language reasoning demonstrations. (Left) Multimodal inputs; (Center) High-level intention inference; (Right) [cite_start]Control execution[cite: 356].</em>
</p>

---

## 📌 Citation

If you use this framework or methodology in your research, please cite the following paper:

```bibtex
@article{pu2026llmdriver,
  title={LLM-Driver: A Vision–Language–Enhanced Multimodal Reasoning Framework for Vehicle Simulation},
  author={Pu, Qingwen and Xie, Kun and Liu, Yuxiang},
  journal={Transportation Informatics Lab, Old Dominion University},
  year={2026},
  note={Under Review}
}
