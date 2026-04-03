# Awesome-Robotics-Embodied-AI
My notes on robotics and embodied AI. Those highlights are my recommendation.

[toc]

# Change Log

- 2026-04-03: docs(README.md): rearrange the article structure into function aspect, add many contents.

---

# Manipulation & Grasping (VLA & Foundation Models & Policy & End-to-End)

## RT-1: Robotics Transformer for Real-World Control at Scale

- [Website](https://robotics-transformer1.github.io/) | [Paper](https://arxiv.org/abs/2212.06817)
- by Google Robotics, 2022

An early large-scale transformer policy for real-world robot control. It showed that large, multi-task, language-conditioned robot policies can be trained on real robot datasets and generalize across many tabletop tasks. It is a bridge from classic imitation learning toward RT-2 / Open-X / OpenVLA style systems.

## ==RT-2: Vision-Language-Action Models: Transfer Web Knowledge to Robotic Control==

- [Website](https://robotics-transformer2.github.io/) | [Paper](https://robotics-transformer2.github.io/assets/rt2.pdf)
- by Google DeepMind, 2023

A canonical VLA paper. The key idea is to co-finetune a vision-language model on robot data, represent robot actions as tokens, and train on both web-scale VLM data and robotics data.

![rt2 overview](assets/rt2_overview.png)

Robot action becomes another “language”. Internet semantic knowledge can help robots, and VLM priors can transfer into robotic action. It showed emergent semantic behavior and improved performance on unseen tasks and objects. The training data couples the current image, the instruction, and the robot action at each timestep.

![rt2 robot action data](assets/rt2_robot_action_data.png)

Its center of gravity is semantic transfer from web-scale VLMs. That is different from later GEN-style claims centered on robotic physical interaction data. It is also different from RDT-style action-centric design, which cares more about continuous action structure and cross-robot action alignment. Tokenized actions are elegant, but continuous control quality and latency remain central challenges. The historical importance is larger than the current open reproducibility. The project connects RT-1 and PaLM-E and points toward Open-X Embodiment / RT-X.

![rt2 open x embodiment](assets/rt2_open_x_embodiment.png)

## ==Diffusion Policy: Visuomotor Policy Learning via Action Diffusion==

- [Website](https://diffusion-policy.cs.columbia.edu/) | [Paper](https://arxiv.org/abs/2303.04137) | [Code](https://github.com/real-stanford/diffusion_policy)
- by [Cheng Chi](https://cheng-chi.github.io) (Columbia) et al.
- RSS 2023

A key policy learning paper from the recent wave of robot learning. Robot action generation is modeled as a conditional denoising diffusion process.

## Octo: An Open-Source Generalist Robot Policy

- [Website](https://octo-models.github.io/) | [Paper](https://arxiv.org/abs/2405.12213) | [Code](https://github.com/octo-models/octo)
- by Dibya Ghosh (UC Berkeley) et al., 2024

An open-source generalist robot policy trained on heterogeneous robot data. It is designed to be adapted to new robots and tasks efficiently.

It functions as a community baseline with open weights, open training code, fine-tuning support, and practical starting points for researchers. Compared with OpenVLA, Octo feels more policy-centric. Compared with GEN, Octo is more open and reproducible, while GEN makes stronger scaling-law claims.

## OpenVLA: An Open-Source Vision-Language-Action Model

- [Website](https://openvla.github.io/) | [Paper](https://arxiv.org/abs/2406.09246) | [Code](https://github.com/openvla/openvla) | [Model](https://huggingface.co/openvla/openvla-7b)
- by Moo Jin Kim (Stanford) et al., 2024

An open-source VLA project. It is a 7B open-source VLA pretrained on robot episodes from Open X-Embodiment. It offers open checkpoints, a PyTorch training pipeline, and fast adaptation via fine-tuning. It serves as an open VLA baseline.

## ==RDT-1B: a Diffusion Foundation Model for Bimanual Manipulation==

- [Website](https://rdt-robotics.github.io/rdt-robotics/) | [Paper](https://arxiv.org/abs/2410.07864) | [Code](https://github.com/thu-ml/RoboticsDiffusionTransformer) | [Model](https://huggingface.co/robotics-diffusion-transformer/rdt-1b)
- by [Songming Liu](https://csuastt.github.io) (Tsinghua University) et al., 2024

![alt text](assets/rdt_head.png)

An action-centric robot foundation model paper. Officially, it is a 1.2B-parameter diffusion transformer pretrained on 46 datasets with 1M+ episodes and fine-tuned on 6K+ self-collected ALOHA bimanual episodes.

![alt text](assets/rdt_framework.png)

It focuses on bimanual manipulation, which is substantially harder than many single-arm settings. It takes the continuous action problem seriously, treats multimodal action generation as first-class, and explicitly targets bimanual dexterity and embodiment heterogeneity. It introduces a Physically Interpretable Unified Action Space.

A unified action space gives a route to cross-robot pretraining, transferable manipulation knowledge, and larger shared action datasets. It is especially relevant for dexterous manipulation, bimanual coordination, cross-embodiment action learning, and real robot fine-tuning after large-scale pretraining. It feels closer to a robotics-native action foundation model than a VLM adapted to robotics.

Compared with RT-2 and many VLA works, it starts from robot action, embodiment, and multimodality itself. The public ecosystem is improving, but it is still less convenient than openpi or OpenVLA as a full end-to-end community stack.

## RDT2: Exploring the Scaling Limit of UMI Data Towards Zero-Shot Cross-Embodiment Generalization

- [Website](https://rdt-robotics.github.io/rdt2/) | [Paper](https://arxiv.org/abs/2602.03310) | [Model](https://huggingface.co/robotics-diffusion-transformer/RDT2-VQ)
- by Songming Liu (Tsinghua University) et al., 2026

A major evolution from RDT-1B. It shifts toward zero-shot cross-embodiment transfer, larger-scale data collection, UMI, and VLA-like structure on top of action modeling.

The project page emphasizes 10,000+ hours of data, 100+ indoor scenes, a 7B VLA backbone, Residual VQ action tokenization, and pretraining from Qwen2.5-VL-7B-Instruct. It merges action-centric robot foundation model thinking from RDT with modern VLA / language-conditioned model design. Data interface design may be as important as model design in embodied AI. In robotics, the collection interface is not just tooling; it can define what kind of scaling becomes possible.

## ==π₀: Our First Generalist Policy==

- [Website](https://www.physicalintelligence.company/blog/pi0) | [Paper](https://www.physicalintelligence.company/download/pi0.pdf) | [Code](https://github.com/Physical-Intelligence/openpi)
- by Karol Hausman (Physical Intelligence) et al., 2024

A robot foundation model built from a pretrained VLM backbone plus a continuous action expert with flow matching for low-level action generation. The goal is a generalist robot policy.

It combines web-scale semantic knowledge, large-scale robot data, continuous low-level motor output, and multi-robot generality. The project emphasizes pretraining on 10,000+ hours of robot data, multiple embodiments, and fine-tuning to dexterous real-world tasks such as folding laundry, clearing tables, putting dishes in microwaves, assembling boxes, and bagging groceries.

Compared with RT-2, it treats low-level action generation as a continuous generative modeling problem. Compared with RDT, it is more clearly built as a VLM-derived VLA foundation model. It is a strong example of combining multimodal pretraining semantics with continuous action generation.

## π₀.₅: a VLA with Open-World Generalization

- [Website](https://www.physicalintelligence.company/blog/pi05) | [Paper](https://www.physicalintelligence.company/blog/pi05) | [Code](https://github.com/Physical-Intelligence/openpi)
- by Ted Xiao (Physical Intelligence) et al., 2025

It pushes beyond familiar setups toward generalization in new homes and new environments. Its main principle is co-training on heterogeneous data.

The training mixture includes robot action data, high-level semantic robot examples, cross-embodiment data, multimodal web data, object detection, and verbal instruction demonstrations. It tries to teach the model not only how to move, but also what the task means, which objects matter, and what subtask should come next.

It uses discrete decoding for high-level semantic actions and continuous flow-matching decoding for low-level motor commands. The model effectively predicts the next subtask before emitting motor control. It is a strong statement that open-world robot generalization requires heterogeneous supervision, not just more demos of the same task. Generalization is a meaningful step, not a solved problem.

## openpi

- [Website](https://www.physicalintelligence.company/blog/openpi) | [Code](https://github.com/Physical-Intelligence/openpi)
- by Physical Intelligence, 2025

A practical repo in embodied AI. It includes pi0, pi0-FAST, pi0.5, checkpoints, inference examples, a fine-tuning pipeline, remote policy serving, and JAX plus later PyTorch support.

It is a serious research and development stack with configs, data conversion, normalization stats, a policy server, and examples for DROID / ALOHA / LIBERO / UR5. It is unusually useful for replication and adaptation. It feels like a real usable research platform instead of a paper artifact.

## ==GEN-0: Embodied Foundation Models That Scale with Physical Interaction==

- [Website](https://generalistai.com/blog/nov-04-2025-GEN-0)
- by Generalist AI, 2025

A high-profile embodied AI announcement whose main significance is the public scaling-law claim.

Robotics is described as entering a scaling-law regime, with real physical interaction data as the key fuel. It argues that there is an intelligence threshold in model scale and that embodied foundation models can be trained directly on high-fidelity raw physical interaction.

Highlighted ideas include Harmonic Reasoning, Cross-Embodiment, 270,000+ hours of real-world manipulation data, a 7B+ phase transition or intelligence threshold, and downstream gains that follow power-law-like scaling. It tries to redefine the field around large-scale physical pretraining.

If that line holds, the center of gravity shifts from benchmark tricks, isolated tasks, and architecture-only novelty toward data engines, scale, embodiment-agnostic pretraining, and post-training efficiency. The announcement attracted attention because it sounds like a robotics analogue of the early GPT scaling moment.

It should currently be treated as a frontier closed research statement rather than an open reproducible baseline. Public information is still much thinner than openpi / OpenVLA / GR00T.

## ==GEN-1: Scaling Embodied Foundation Models to Mastery==

- [Website](https://generalistai.com/blog/apr-02-2026-GEN-1)
- by Generalist AI, 2026

It moves the story from scaling exists to scaled embodied models approaching economically meaningful usefulness. Publicly emphasized themes include reliability, speed, improvisation / recovery, and a much larger data engine, reportedly now over 500,000 hours.

Many robot demos fail because success rate is too low, execution is too slow, or recovery is too weak. The public story directly targets those issues. If GEN-0 is the scaling-law declaration, GEN-1 is the commercial or deployment threshold declaration.

The field has many strong demos but fewer signs of robust repetition, useful speed, and spontaneous recovery in messy edge cases. The public narrative is about those missing ingredients. It still lacks the same full technical transparency as openpi / Octo / OpenVLA / GR00T. It should be treated as very important and high-signal, but still much less independently reproducible.

---

# Perception & Computer Vision & VLM

Outstanding work from perception and computer vision that is frequently used in robotics pipelines.

## SAM2

- [Website](https://ai.meta.com/sam2/) | [Paper](https://arxiv.org/abs/2408.00714) | [Code](https://github.com/facebookresearch/sam2)
- by Nikhila Ravi / Meta, 2024

Segment Anything Model 2 for image and video segmentation and mask generation. Useful as a perception primitive in embodied systems.

## DINOv2

- [Website](https://ai.meta.com/blog/dino-v2-computer-vision-self-supervised-learning/) | [Paper](https://arxiv.org/abs/2304.07193) | [Code](https://github.com/facebookresearch/dinov2)
- by Maxime Oquab / Meta, 2023

A strong visual representation model for feature extraction and perception.

## Grounding DINO / Grounded SAM

- [Website](https://github.com/IDEA-Research/GroundingDINO) | [Paper](https://arxiv.org/abs/2303.05499) | [Code](https://github.com/IDEA-Research/GroundingDINO) | [Grounded-SAM](https://github.com/IDEA-Research/Grounded-Segment-Anything)
- by Shilong Liu / IDEA Research, 2023-2024

Useful for open-vocabulary detection and segmentation. A strong perception module for object-centric robot pipelines.

## GPT-4o

- [Website](https://openai.com/index/gpt-4o-and-more-tools-to-chatgpt-free/) | [Model](https://openai.com/index/hello-gpt-4o/)
- by OpenAI, 2024

A strong multimodal reasoning model. Useful for planning, semantic grounding, scene understanding, and agentic robot stacks.

## GraspNet / AnyGrasp

- [Website](https://graspnet.net/) | [Paper](https://arxiv.org/abs/1912.13470) | [Code](https://github.com/graspnet/graspnet-baseline) | [AnyGrasp](https://graspnet.net/anygrasp.html)
- by Hao-Shu Fang (SJTU) et al., 2020

Strong grasp-related perception and model families. More relevant for grasp synthesis than full embodied policy. Dexterous hand-specific foundation models remain an open gap.

TODO: what are the most important grasp-related foundation models specifically for dexterous hands?

---

# Planning & Reasoning (Agent & modular systems)

## ==SayCan: Do As I Can, Not As I Say: Grounding Language in Robotic Affordances==

- [Website](https://say-can.github.io) | [Paper](https://say-can.github.io/assets/palm_saycan.pdf) | [Code](https://github.com/google-research/google-research/tree/master/saycan)
- by Michael Ahn / Google, 2022

An early “LLM for robot task planning” work. It combines language-model probability with a value function or affordance score, then selects a skill to execute.

![saycan decision making](assets/saycan_decision_making.png)

The split between semantic usefulness and physical feasibility is the key template here. A skill should be both useful and possible.

![saycan language and affordance](assets/saycan_language_and_affordance.png)

It is not yet an end-to-end generalist policy. It depends on a fixed skill library.

## VoxPoser: Composable 3D Value Maps for Robotic Manipulation with Language Models

- [Website](https://voxposer.github.io) | [Paper](https://arxiv.org/abs/2307.05973) | [Code](https://github.com/huangwl18/VoxPoser)
- by Wenlong Huang (Stanford) et al., 2023

Builds 3D value maps for language-guided manipulation. It is more planner and grounding oriented than foundation-model pretraining oriented. It is still conceptually useful for geometric affordance grounding.

## ReKep: Spatio-Temporal Reasoning of Relational Keypoint Constraints for Robotic Manipulation

- [Website](https://rekep-robot.github.io) | [Paper](https://arxiv.org/abs/2409.01652) | [Code](https://github.com/huangwl18/ReKep)
- by Wenlong Huang (Stanford) et al., 2024

Uses modern perception modules plus reasoning to generate keypoints, constraints, and subgoals. It is meaningful for structured manipulation reasoning rather than pure end-to-end policy learning.

---

# Locomotion (Humanoid & Legged)

## Humanoid

### TWIST: Teleoperated Whole-Body Imitation System

- [Website](https://yanjieze.com/TWIST/) | [Paper](https://arxiv.org/abs/2505.02833) | [Code](https://github.com/YanjieZe/TWIST)
- by [Yanjie Ze](https://yanjieze.com) (Stanford) et al., 2025

A whole-body humanoid teleoperation system through motion imitation. It emphasizes coordinated loco-manipulation rather than isolated walking or isolated arm control.

### TWIST2: Scalable, Portable, and Holistic Humanoid Data Collection System

- [Website](https://yanjieze.com/TWIST2/) | [Paper](https://arxiv.org/abs/2511.02832) | [Code](https://github.com/amazon-far/TWIST2)
- by Yanjie Ze (Amazon FAR / Stanford) et al., 2025

A follow-up system focused on scalable whole-body humanoid data collection. The emphasis is not only control, but also building a practical data engine for humanoid visuomotor learning.

### OmniH2O: Universal and Dexterous Human-to-Humanoid Whole-Body Teleoperation and Learning

- [Website](https://omni.human2humanoid.com) | [Paper](https://arxiv.org/abs/2406.08858) | [Code](https://github.com/LeCAR-Lab/human2humanoid)
- by [Tairan He](https://tairanhe.com) (CMU) et al., 2024

A whole-body humanoid teleoperation and autonomy framework. It supports VR, RGB-camera-based teleoperation, and learning from demonstrations on full-sized humanoids with dexterous hands.

### Learning Human-to-Humanoid Real-Time Whole-Body Teleoperation

- [Website](https://human2humanoid.com) | [Paper](https://arxiv.org/abs/2403.04436) | [Code](https://github.com/LeCAR-Lab/human2humanoid)
- by Tairan He (CMU) et al., 2024

A reinforcement-learning-based framework for real-time whole-body humanoid teleoperation using only RGB input. It is a representative early system for learning-based full-body humanoid teleop.

## Legged

### Learning Quadrupedal Locomotion over Challenging Terrain

- [Website](https://leggedrobotics.github.io/rl-blindloco/) | [Paper](https://www.science.org/doi/10.1126/scirobotics.abc5986) | [Code](https://github.com/leggedrobotics/legged_gym)
- by Joonho Lee (ETH Zurich) et al., 2020

A landmark RL locomotion paper for quadrupeds. It demonstrated blind locomotion over challenging terrain and strongly influenced later legged RL pipelines.

### Rapid Locomotion via Reinforcement Learning

- [Website](https://agility.csail.mit.edu/) | [Paper](https://arxiv.org/abs/2205.02824) | [Code](https://github.com/Improbable-AI/rapid-locomotion-rl)
- by Gabriel Margolis (MIT) et al., 2022

A representative high-speed quadruped locomotion work. It focuses on agility, turning, and robust real-world transfer on the MIT Mini Cheetah.

### Agile But Safe: Learning Collision-Free High-Speed Legged Locomotion

- [Website](https://tairanhe.com/) | [Paper](https://arxiv.org/abs/2401.17583) | [Code](https://github.com/LeCAR-Lab/ABS)
- by Tairan He (CMU) et al., 2024

A legged locomotion framework that emphasizes both agility and safety. The key point is not only moving fast, but doing so while remaining collision-aware in cluttered environments.

---

# Navigation (SLAM & Path Planning & End to End & VLN)

## ==ORB-SLAM3==

- [Paper](https://arxiv.org/abs/2007.11898) | [Code](https://github.com/UZ-SLAMLab/ORB_SLAM3)
- by Carlos Campos (University of Zaragoza) et al., 2020

A classic open-source SLAM system for visual, visual-inertial, and multi-map SLAM. Still a very important baseline in robotics and embodied navigation.

## VINS-Fusion

- [Website](https://github.com/HKUST-Aerial-Robotics/VINS-Fusion) | [Code](https://github.com/HKUST-Aerial-Robotics/VINS-Fusion)
- by Tong Qin (HKUST) et al., 2019

An optimization-based multi-sensor state estimator. A classic line in visual-inertial state estimation and robotics localization.

## FAST-LIO2

- [Paper](https://arxiv.org/abs/2107.06829) | [Code](https://github.com/hku-mars/FAST_LIO)
- by Wei Xu (HKU MARS Lab) et al., 2021

A fast and accurate LiDAR-inertial odometry and mapping framework. A very important line in modern robotics navigation and mapping.

## OK-Robot

- [Website](https://ok-robot.github.io/) | [Paper](https://arxiv.org/abs/2401.12202) | [Code](https://github.com/ok-robot/ok-robot)
- by Peiqi Liu (NYU) et al., 2024

Uses multiple foundation models for home robot navigation, perception, and manipulation. It is more system integration oriented and serves as a practical example of a multi-model embodied stack.

---

# Datasets & Benchmarks

## ==Open X-Embodiment==

- [Website](https://robotics-transformer-x.github.io/) | [Paper](https://arxiv.org/abs/2310.08864) | [Code](https://github.com/google-deepmind/open_x_embodiment)
- by Open X-Embodiment Collaboration, 2023

![oxe_overview](assets/oxe_overview.png)

A large open multi-robot data effort. It helped normalize the idea that embodied learning needs heterogeneous multi-robot datasets, not isolated single-lab datasets. It strongly influenced Octo, OpenVLA, and open ecosystem training recipes.

## DROID: A Large-Scale In-The-Wild Robot Manipulation Dataset

- [Website](https://droid-dataset.github.io/) | [Paper](https://arxiv.org/abs/2403.12945)
- by Alexander Khazatsky (Stanford) et al., 2024

A large real-world manipulation dataset collected across many scenes, tasks, and data collectors. It pushes robot data collection out of a single lab and into a more distributed real-world regime. It is also a practical dataset for post-training and evaluation in the open ecosystem.

## BridgeData V2: A Dataset for Robot Learning at Scale

- [Website](https://bridgedata-v2.github.io/) | [Paper](https://arxiv.org/abs/2308.12952)
- by Homer Walke (UC Berkeley) et al., 2023

A large manipulation dataset on a low-cost platform with strong task and environment diversity. It is useful as a practical benchmark and data source for scalable robot learning and for open-source policy pipelines such as OpenVLA.

## AgiBot World / AgiBot World Colosseo

- [Website](https://agibot-world.com/) | [Paper](https://arxiv.org/abs/2503.06669) | [Code](https://github.com/OpenDriveLab/Agibot-World) | [Dataset](https://huggingface.co/agibot-world/datasets) | [Model](https://huggingface.co/agibot-world/GO-1)
- by Qingwen Bu (AgiBot World) et al., 2025

A large-scale platform and dataset line oriented toward generalized manipulation, dexterous skills, and long-horizon embodied learning. Public materials emphasize 1M+ trajectories, 217 tasks, and large-scale data collection with both real-world and digital-world components.

## LIBERO

- [Website](https://libero-project.github.io/main.html) | [Paper](https://arxiv.org/abs/2306.03310) | [Code](https://github.com/Lifelong-Robot-Learning/LIBERO)
- by Bo Liu (UT Austin) et al., 2023

A benchmark suite for lifelong and compositional robot learning in simulation. It is widely used for controlled evaluation of policy and VLA methods, especially for fine-tuning and benchmark comparison.

---

# Hardware

## Humanoid

* [Unitree G1](https://www.unitree.com/en/g1)
* [AGIBOT A2 / X1 / X2](https://www.agibot.com.cn/)

## Robotic Arms

* [Franka Research 3 / Panda](https://www.franka.de/)
* [Universal Robots UR Series](https://www.universal-robots.com/products/)
* [Galaxea A1 / XA1](https://galaxea-ai.com/cn/about)

## Dexterous Hand

* [Shadow Dexterous Hand](https://shadowrobot.com/)
* [Sharpa / SharpaWave](https://www.sharpa.com/)
* [Wuji Hand](https://www.wuji.tech/)
* [ORCA Hand](https://srl.ethz.ch/platforms/srh/orcahand.html)
* [Aero Hand Open / AeroHand](https://tetheria.github.io/aero-hand-open/)

## Wheeled Robot

* [Galaxea R1 / R1 Pro / R1 Lite](https://galaxea-ai.com/cn/about)
* [AGIBOT C5 / G1 / G2](https://www.agibot.com.cn/)
* [Galbot](https://www.galbot.com/)

---

# Simulator

## Isaac Lab

- [Website](https://developer.nvidia.com/isaac/lab) | [Code](https://github.com/isaac-sim/IsaacLab)
- by NVIDIA

The current open-source unified robot learning framework in NVIDIA Isaac. It is built on Isaac Sim and is the natural successor to Isaac Gym for many robot learning workflows.

## MuJoCo

- [Website](https://mujoco.org/) | [Code](https://github.com/google-deepmind/mujoco)
- by Google DeepMind

A general-purpose physics simulator for articulated dynamics and contact. Still one of the most widely used simulators in robotics, control, and reinforcement learning.

---

# Recommended Resources

1. [Springer Handbook of Robotics, 2nd Edition](https://link.springer.com/referencework/10.1007/978-3-319-32552-1)
   A broad robotics reference that is still useful.

2. [Introduction to Robotics, 4th Edition](https://www.amazon.com/Introduction-to-Robotics-Global-Edition/dp/129216493X)
   Good classical background.

3. [Modern Robotics](https://hades.mech.northwestern.edu/index.php/Modern_Robotics)
   Good textbook for learning robotics.

4. [LeRobot](https://huggingface.co/docs/lerobot)
   Hugging Face open robotics ecosystem.

5. [机器人工程师学习计划](https://zhuanlan.zhihu.com/p/22266788)
   Specially Thanks to Shuo Yang. This article guided me into robotics in 2022.

---

# TODO

[ ] add learning notes on ACT / Gemini Robotics / Helix(Figure AI)
[ ] add dexterous hand algorithm like dexgraspvla, teleoperation and retargeting
[ ] add notes on data engines like teleoperation, wearables (UMI, MANUS gloves, etc.,), synthetic trajectories and egocentric human video
[ ] add more VLN / navigation entries

---

# Contributing

Feel free to share recommendations and thoughts via PR or Issues.

[Mengyang Gao 高梦扬](https://github.com/MengyangGao)