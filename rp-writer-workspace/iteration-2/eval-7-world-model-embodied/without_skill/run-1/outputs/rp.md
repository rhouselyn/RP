# Latent-Generative World Models for Sample-Efficient and Transferable Robotic Manipulation

**A Research Proposal for Doctoral Study**

---

## 1. Introduction

Embodied agents must reason about the consequences of their actions in order to manipulate objects skillfully. World models—learned predictors of environment dynamics—have emerged as a powerful paradigm, enabling planning, imagination-based reinforcement learning (RL), and sample-efficient adaptation. Two recent currents have brought world models to the forefront of embodied AI. On one hand, breakthroughs in generative modeling, exemplified by video diffusion predictors, produce striking, high-fidelity forecasts of future observations. On the other hand, latent world models such as DreamerV3 learn compact dynamics in latent space and drive state-of-the-art model-based RL with remarkable sample efficiency.

Despite this progress, applying world models to contact-rich robotic manipulation—under the stringent sample-efficiency and sim-to-real transfer requirements of real-world deployment—remains an open problem. Generative models offer fidelity but are sample-hungry, slow, and provide weak control signals; latent models offer control efficiency but sacrifice the visual and contact detail that determines manipulation success. Worse, virtually all world models for manipulation are trained in simulation, and it is unknown whether they—and the policies they imagine—can transfer to real hardware.

This proposal targets a PhD investigating world models for robotic manipulation, situated at the intersection of robot learning and sim-to-real transfer—areas central to my prospective supervisor's research program. I argue that neither purely generative nor purely latent world models alone suffice, and propose a hybrid latent-generative world model that combines the strengths of both, together with methods that explicitly transfer the world model, not merely the policy, from simulation to reality. The work will produce new architectures, training algorithms, and empirical validation on real robotic manipulation.

## 2. Background and Motivation

World models learn a dynamics function over states and actions, allowing an agent to "imagine" rollouts for planning or RL. Two paradigms dominate.

**Generative world models** predict future observations directly in pixel (or point-cloud) space. Recent systems—video diffusion predictors, Genie, UniSim, and IRIS—leverage large-scale pretraining and produce visually diverse, photorealistic forecasts. They are attractive for tasks requiring rich visual reasoning, such as navigation or scene forecasting. For manipulation, however, generating pixel-accurate futures is computationally expensive and sample-hungry, and—critically—does not directly yield actionable control: gradients through high-dimensional decoders are noisy, and contact events, where dynamics change discontinuously, are notoriously hard to render faithfully.

**Latent world models** instead learn dynamics in a compact latent space, often via recurrent state-space models (RSSMs) as in Dreamer and DreamerV3, or via masked latent prediction as in JEPA-style approaches. They enable efficient actor-critic learning entirely in imagination and have achieved strong results on Atari, DMC, and Crafter. For manipulation, their compactness supports long-horizon planning and fast policy improvement. Yet they typically sacrifice fine-grained visual detail and struggle to represent contact-rich phenomena—forces, deformations, and tactile cues—that determine manipulation success.

Meanwhile, robot learning increasingly relies on simulation for data volume, but transferring policies to real hardware (sim-to-real) suffers from the reality gap. Standard remedies—domain randomization, system identification, and real-data fine-tuning—are designed for *policies*, not for the *world models* that generate imagined experience. Whether and how world models themselves can be made transferable is largely unstudied.

This proposal is motivated by three observations. First, manipulation demands both control efficiency and contact/visual fidelity, suggesting neither paradigm suffices alone. Second, the sim-to-real gap affects not only the policy but the imagined dynamics on which the policy is trained, making world-model transfer a first-class problem. Third, the supervisor's expertise in robot learning and sim-to-real provides the ideal setting to attack these questions end-to-end, from simulated training to real-robot evaluation.

## 3. Research Questions

I will investigate three interlocking questions:

**RQ1 (Architecture).** Can a hybrid latent-generative world model jointly deliver sample-efficient control (from latent dynamics) and high-fidelity, contact-aware prediction (from a generative head), outperforming either paradigm alone on manipulation tasks?

**RQ2 (Sim-to-real transferability).** How can a world model trained predominantly in simulation be adapted to real-robot dynamics with limited real data, and does transferring the world model—rather than only the policy—improve real-world performance?

**RQ3 (Long-horizon reasoning).** Can the proposed world model support long-horizon, contact-rich manipulation via imagination-based planning and goal-conditioned RL, including tasks with tool use and multi-step object rearrangement?

## 4. Related Work

**Model-based RL and latent world models.** Dreamer established imagination-based actor-critic learning from RSSM posteriors; DreamerV3 scaled this across domains with fixed hyperparameters. TD-MPC performs latent-space model-predictive control. JEPA and V-JEPA advocate predictive latent objectives without pixel generation, arguing that generative decoders waste capacity on irrelevant detail. These works target games or locomotion; their behavior on contact-rich manipulation is underexplored.

**Generative world models.** Video diffusion (Sora-style predictors), GAIA, Genie, and UniSim generate future video conditioned on action or text; recent work conditions generation on robot actions to forecast manipulation outcomes. These provide rich prediction but are slow, data-hungry, and not directly coupled to control.

**World models for manipulation.** Dreamer has been applied to simple manipulation; recent work adds tactile inputs or point-cloud observers. However, hybrid architectures that explicitly combine latent control dynamics with generative fidelity are nascent, and their sim-to-real behavior is essentially unstudied.

**Sim-to-real for robot learning.** Domain randomization, system identification, and teacher–student distillation are standard; recent work uses real-data fine-tuning via RL or diffusion policies. The transfer of *world models*—rather than policies—is an emerging but open direction.

This proposal contributes a unified treatment of these threads for manipulation, with explicit attention to the sim-to-real behavior of the model itself.

## 5. Proposed Approach

### 5.1 Hybrid latent-generative architecture

I propose a world model with a latent dynamics core and a generative prediction head, sharing a perceptual encoder. The latent core follows an RSSM that maintains a recurrent hidden state, deterministic and stochastic posteriors, and predicts the next latent from the current latent and action. Actor and critic are learned entirely in latent imagination, as in DreamerV3, providing sample-efficient control. A diffusion-based generative head, conditioned on sampled latent states, predicts future RGB-D frames at selected horizons.

Crucially, the generative head is trained with an *auxiliary consistency objective*: decoding rolled-out latents should match the generative prediction, and conversely the latent posterior should agree with the encoding of the generative forecast. This coupling lets the latent model borrow representational capacity from the generative head, while the generative head inherits the latent model's temporal coherence and action-conditioning. Ablations will isolate the contribution of each component and of the consistency coupling.

### 5.2 Contact-aware latent state

Manipulation success hinges on contacts. I will augment the latent state with force/torque and tactile signals (when available) and with a learned "contact token" that gates transition dynamics, enabling discontinuity-aware prediction. For settings without tactile sensing, I will infer pseudo-contact from visual cues via a small detector trained on simulated contact labels, then distill it into the latent state. The generative head will additionally be conditioned on the contact token to improve contact-region fidelity. The hypothesis is that explicit contact representation narrows the class of dynamics the model must learn, improving both prediction accuracy and downstream control.

### 5.3 Sim-to-real transfer of world models

The core sim-to-real hypothesis is that a *transferable world model* yields a *transferable policy*, because the policy is trained against the model's imagined dynamics. I will pursue three complementary mechanisms:

1. **Latent-space domain randomization.** Beyond randomizing visual and physical parameters in simulation, I will randomize the latent transition itself—perturbing latent dynamics within a learned uncertainty bound—so that policies are robust to model misspecification. This generalizes standard domain randomization to the model's internal dynamics.
2. **Real-data latent fine-tuning.** Using a modest real-robot dataset (~1–5k transitions) without rewards, I will fine-tune the encoder and latent transition via a contrastive/predictive objective, aligning latent rollouts with real observations while keeping the generative head as a fixed regularizer to prevent catastrophic forgetting.
3. **Residual dynamics correction.** A small residual network will correct simulated latent transitions to match real ones, trained on paired simulated–real transitions. This mirrors residual physics but in latent space, lowering the data burden because corrections operate on compact states rather than pixels.

Each mechanism will be evaluated individually and in combination, against the baseline of transferring only the policy.

### 5.4 Long-horizon planning and goal-conditioned RL

For long-horizon tasks (tool use, multi-object rearrangement), I will combine imagination-based RL with model-predictive planning. The latent model supports long rollouts for policy improvement; the generative head supports goal-conditioned planning by predicting whether imagined trajectories reach visual goal states. A hierarchical objective—high-level sub-goals proposed from the generative head, low-level control executed by the latent actor—will be evaluated on multi-step benchmarks. This design directly leverages the hybrid's complementary strengths: the generative head for coarse visual goal evaluation, the latent core for efficient control synthesis.

### 5.5 Experimental plan

**Simulation.** MetaWorld, RLBench, and a contact-rich suite (e.g., soft-body or articulated-object tasks) for ablation and baselines (DreamerV3, TD-MPC, V-JEPA-style predictor, diffusion-only predictor). Metrics: task success, sample efficiency, prediction error (PSNR/LPIPS for the generative head; latent NLL), and real-world transfer success.

**Real robot.** A Franka Emika arm with wrist-mounted RGB-D and optional tactile fingertips, evaluated on three to four contact-rich tasks (e.g., precision insertion, drawer opening, cloth folding). Comparison to baselines trained directly in simulation and to real-data-only imitation, measuring real-world success rate and data efficiency. All experiments will include ablations isolating the contact token, the consistency coupling, and each sim-to-real mechanism.

## 6. Research Plan and Timeline

**Year 1 — Foundations (months 1–12).** Deep literature study; reproduce DreamerV3 and a diffusion world-model baseline on MetaWorld; implement the latent core with contact tokens; first ablations on contact-rich simulated tasks. *Deliverable:* a workshop or conference submission on contact-aware latent world models for manipulation.

**Year 2 — Hybrid architecture and sim-to-real (months 13–24).** Develop and validate the hybrid latent-generative model; implement latent-space domain randomization and residual correction; begin real-robot data collection. *Deliverable:* a top-tier conference submission on the hybrid model and sim-to-real transfer of world models.

**Year 3 — Real-robot validation and long-horizon tasks (months 25–36).** Full real-robot evaluation; hierarchical long-horizon planning experiments; comparison with state-of-the-art model-based and model-free baselines. *Deliverable:* a second top-tier submission on real-world contact-rich manipulation with world models.

**Year 4 — Synthesis and extension (months 37–48).** Extension to multi-modal goals (language-conditioned manipulation) and deformable objects; completion of thesis writing; internship collaboration with the supervisor's industrial partners where applicable.

## 7. Expected Contributions

This work is expected to deliver:

1. A **hybrid latent-generative world-model architecture** that unifies control efficiency and prediction fidelity for manipulation, with a consistency-coupling objective that lets the two components supervise one another.
2. A **suite of sim-to-real techniques specifically targeting world models**—latent-space domain randomization, real-data latent fine-tuning, and residual dynamics correction—establishing empirically whether transferable world models yield transferable policies.
3. **Empirical evidence on contact-rich, long-horizon manipulation tasks** in both simulation and on real hardware, with rigorous ablations against latent-only, generative-only, and policy-only-transfer baselines.
4. **Open-source code and pretrained models** to support reproducibility and follow-on research within the supervisor's group and the broader community.

## 8. Fit with the Research Group

The proposal aligns directly with the supervisor's research program in robot learning and sim-to-real transfer. World models are a natural substrate for robot learning: they convert expensive real interaction into cheap imagined experience. By explicitly addressing the transferability of the world model—rather than only the policy—this work extends the group's sim-to-real agenda to the model itself, a direction with both intellectual novelty and practical value for deployment. The planned real-robot evaluation leverages the group's manipulation hardware and expertise, while the simulation-first methodology keeps data costs tractable. I am particularly excited to contribute rigorous, model-based methods to a lab already strong in these areas, and to benefit from mentorship at the simulation-to-deployment frontier that defines modern robot learning.

## Selected References

- Hafner, D., et al. *Mastering Diverse Domains through World Models* (DreamerV3). 2023.
- Hafner, D., et al. *Dream to Control: Learning Behaviors by Latent Imagination* (Dreamer). 2020.
- Hansen, N., et al. *TD-MPC: Temporal Difference Learning for Model Predictive Control*. 2022.
- Assran, M., et al. *Self-Supervised Learning from Images with a Joint-Embedding Predictive Architecture* (V-JEPA). 2024.
- Bruce, J., et al. *Genie: Generative Interactive Environments*. 2024.
- Yang, D., et al. *UniSim: Learning Interactive Real-World Simulators*. 2023.
- Bakhtin, A., et al. *GAIA-1: A Generative World Model for Autonomous Driving*. 2023.
- Micheli, V., et al. *Transformers are Sample-Efficient World Models* (IRIS). 2023.
- Tobin, J., et al. *Domain Randomization for Transferring Deep Neural Networks from Simulation to the Real World*. 2017.
- Zhao, W., et al. *Sim-to-Real Transfer in Deep Reinforcement Learning for Robotics: a Survey*. 2020.

---

*Word count (main text, Sections 1–8, excluding title, references, and headings): approximately 1,980 words.*
