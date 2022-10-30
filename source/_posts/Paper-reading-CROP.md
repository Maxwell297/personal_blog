---
title: Paper reading - CROP
cover: false
toc: true
mathjax: true
date: 2022-10-30 15:50:12
author: Maxwell Yao
password:
summary:
tags: 
- RL
- CS
categories: Theoretical RL
---

# Paper reading | CROP

`CROP: CERTIFYING ROBUST POLICIES FOR REINFORCEMENT LEARNING THROUGH UNCTIONALSMOOTHING` by `Fan Wu et al.`.

## Introduction

- This paper asks two questions mainly:
  - How to provide efficient and effective robustness certification for RL algorithms?
  - What criteria should be used to certify the robustness of RL algorithms?
- Focus on Q-learning with two certification criteria
  - Per-state action action stability
  - Lower bound of perturbed cumulative reward

- Contributions of this paper

  - Propose a framework for certifying the robustness of Q-learning algorithms

    - 2 Robustness certification criteria

  - Prove the certification radius for input state and lower bound of perturbed cumulative reward under bounded adversarial state perturbations

  - Conduct extensive experiments to provide certification for nine empirically robust RL

    algorithms on multiple RL environments



## Preliminaries

- Discounted discrete-time MDPs
  - Defined by $(\mathcal{S,A},R,P,\gamma,d_0)$, which represent states, discrete actions, reward function, transition function, discount factor and initial state distribution respectively.



## Robustness certification in Q-learning

- Consider the standard adversarial setting in Q-learning, where the adversary can apply $l_2$ bounded perturbation $\mathcal B^{\varepsilon}$ to input state observations of the agent during decision time.

### Robustness certification for Q-learning with different criteria

- **Def. 1.** Given a trained network $Q^\pi$, define the robustness certification for per-state action as the *maximum perturbation magnitude* $\bar \varepsilon$, s.t. for any perturbation $\delta\in\mathcal B^{\bar\varepsilon}$, the predicted action under the perturbed state will be the same as the action taken in the clean environment, i.e., $\pi(s+\delta)=\pi(s),\forall \delta\in\mathcal B^{\bar\epsilon}$.

- **Def. 2.** Define the *perturbed cumulative reward* as following
  $$
  J_{\varepsilon}(\pi)\triangleq \sum_{t=0}^\infty\gamma^t R(s_t,\pi(s_t+\delta_t)),\text{ where }s_{t+1}\sim P(s_t,\pi(s_t+\delta_t)),s_0\sim d_0
  $$

- **Def. 3.** The robustness certification of cumulative reward is the *lower bound of perturbed cumulative reward* $\underline J$ s.t. $\underline J\le J_{\varepsilon}(\pi)$ under the perturbation in $\mathcal B^\varepsilon$ applied to all time steps.



## Robustness certification strategies for per-state action

### Action-value functional smoothing

- Given $Q^\pi$ with policy $\pi$, derive a smoothed function $\tilde Q^\pi$ through per-state *local smoothing*.

  - At each time step $t$, for each action $a\in \mathcal A$, we draw random noise from a Gaussian distribution $\mathcal N(0,\sigma^2I_N)$ and do

    {% asset_img image-1.png Per-state local smoothing %}

- Then based on the definition of smoothness, we have the following theorems
  - {% asset_img image-2.png Lemma 1 %}
  - {% asset_img image-3.png Theorem 1 %}

### Local randomized smoothing

- Challenges for Q-learning compared with standard classification task.

  - Unknown range $[V_{\min},V_{\max}]$

    - Do pre-processing: estimate the output range based on a finite set of valid states

  - For Q-networks, the outputs aren't probabilities, and calculating the multinomial proportions becomes challenging.

    - *Hoeffding's inequality*

      

## Robustness certification strategies for the cumulative reward

### Global smoothing

- Perform *global smoothing* on the state trajectory by viewing the entire trajectory as a function.

- **Def. 4.** 
