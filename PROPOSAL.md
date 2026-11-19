# Project Proposal: How Robust Is Diffusion Policy to Camera Corruption?

**Track:** A (Research): reproduce and extend
**Student:** Hassaan Sajid · **Roll no.:** 582738 · **Repository:** https://github.com/hassaanSajid97/diffusion-policy-robustness

## 1. Benchmark paper

C. Chi, S. Feng, Y. Du, Z. Xu, E. Cousineau, B. Burchfiel, and S. Song, "Diffusion Policy: Visuomotor Policy Learning via Action Diffusion," in *Proceedings of Robotics: Science and Systems (RSS)*, Daegu, Republic of Korea, July 2023.

- **Paper:** https://roboticsproceedings.org/rss19/p026.html (arXiv: https://arxiv.org/abs/2303.04137v4)
- **Venue:** RSS, which the course guideline lists as an accepted top venue. CORE rank: A* (CORE2018; listed as "to be ranked" in CORE2023) · Google Scholar citations: 4,290 (checked on 19 Nov 2026; Scholar groups this with the extended journal version, IJRR 2025)
- **Official code, data and checkpoints:** https://github.com/real-stanford/diffusion_policy
- **Pretrained Push-T checkpoint (LeRobot re-implementation):** https://huggingface.co/lerobot/diffusion_pusht
- **Simulator:** gym-pusht, https://github.com/huggingface/gym-pusht

## 2. Result to reproduce

**Table II (Visual Policy), Push-T column, Diffusion Policy-C (CNN):** target-area coverage **0.91** (best checkpoint) / **0.84** (average of the last 10 checkpoints). Each value is averaged over 3 training seeds × 50 initial conditions.

I will run the publicly released pretrained image-based Diffusion Policy in the simulated Push-T task. The task: a robot pushes a T-shaped block into a target pose using a 96×96 camera image. I will measure average maximum target coverage, the paper's metric, and success rate (≥ 95% coverage) over at least 200 test episodes. No training is needed for the reproduction.

## 3. Research question

> *How much does the pretrained vision-based Diffusion Policy's average target-area coverage on Push-T (paper value 0.91/0.84) drop when its camera images are corrupted by (a) Gaussian pixel noise, (b) motion blur, or (c) an observation delay of 1–3 control steps, at increasing severity levels?*

**Motivation:** real robot cameras produce noisy, blurred and delayed images. The paper evaluates the simulated task only with clean, instant images.

**Measurement:** I will record coverage and success rate for 3 corruption types × 4 severity levels, plus the clean baseline. I will report the relative drop from the clean baseline and the severity at which coverage falls below 0.80.

## 4. Data and compute

- **Data:** no dataset download is needed for evaluation. The policy is pretrained (Hugging Face), and the gym-pusht simulator generates test episodes from fixed random seeds.
- **Compute:** Google Colab (free GPU), running LeRobot or the authors' official Colab notebook; my local NVIDIA RTX A2000 (12 GB) is the backup. The workload is inference only, with no training. Results are saved to Google Drive after every experiment batch, so a Colab disconnect loses at most one batch.
- **Code:** my own Python module wraps the simulator's camera observations with corruption functions (NumPy/OpenCV). An evaluation script runs all experiments from fixed seeds so the results can be reproduced. Includes pytest unit tests.

## 5. Plan

| Date | Milestone |
|---|---|
| **26 Nov** | Pretrained policy running in Push-T on Colab; first rollout video saved |
| **4 Dec (checkpoint)** | **Baseline reproduced:** coverage and success rate over ≥ 200 episodes, compared with the paper's 0.91/0.84; corruption functions written and unit-tested |
| **11 Dec** | Extension experiments finished (3 corruptions × 4 levels); results table and plots |
| **18 Dec (final)** | 4–6 page IEEE paper, clean repository and README, Python skills profile linked |

**Fallback if the vision policy cannot be reproduced:** use the authors' state-based Push-T policy (Table I, coverage 0.95/0.91). The authors provide this version as a self-contained Google Colab notebook. I would apply Gaussian noise and delay to its state input (block and robot positions) instead of to images. The research question stays the same, measured with the same metric.

**Expected discrepancy to explain:** the released checkpoint is a single training run (the paper averages 3), and part of the reproduction uses LeRobot's re-implementation. I will report the paper's numbers, LeRobot's own reported numbers and mine side by side.
