# Reinforcement Learning for Grid Frequency Stabilisation

## Overview

This project investigated the use of reinforcement learning for adaptive frequency regulation in a microgrid under changing and stochastic operating conditions.

A Proximal Policy Optimization (PPO) agent was developed in Python and coupled with a MATLAB/Simulink microgrid simulation. The agent generated a continuous control signal for an inverter control block associated with battery energy storage.

The project focused on the formulation and evaluation of a learning-based frequency regulation controller, with particular attention to state representation, reward design, controller stability, adaptability, and control effort.

## Research Objective

The main objective was to investigate whether a reinforcement-learning controller could maintain microgrid frequency stability under changing operating conditions and stochastic disturbances.

The project examined how a learning-based controller responded to variations in load demand and other system conditions, and how its performance compared with conventional fixed-rule control.

## System

The simulated microgrid included:

- Conventional generation
- Battery energy storage
- Battery inverter
- Inverter control block
- Electrical load
- Microgrid frequency dynamics

The RL controller operated at a supervisory level. The PPO agent generated a continuous control signal that was passed to the inverter control block rather than directly controlling inverter switching.

## Reinforcement Learning

### Algorithm

The controller used Proximal Policy Optimization (PPO) for continuous control.

### State

The refined state representation consisted of:

```text
[frequency deviation,
 previous control action,
 inverter output]
```
The state was reformulated after the initial controller exhibited unstable and inconsistent behaviour. The original state representation was not capturing enough of the system dynamics for the controller to make consistent decisions.

### Action

The agent generated a continuous control signal for the inverter control block.

### Reward

The reward was designed to encourage reduction of frequency deviation while penalising excessive control action.

The exact historical reward equation and weighting coefficients are not available.

## Training Environment

The microgrid simulation was implemented in MATLAB/Simulink, while PPO training was performed in Python.
The project involved TensorFlow and/or PyTorch, although the exact historical role of each framework is no longer available.

The exact mechanism used for communication between Python and MATLAB/Simulink is also unavailable.

## Disturbance Conditions

The controller was exposed to stochastic, time-varying disturbances, including random load-demand variations.

Evaluation considered:

- Different load levels
- Different disturbance magnitudes
- Different stochastic disturbance profiles
- Conditions distinct from those used during training

## Controller Development

The controller was developed iteratively.

### Initial Formulation

The initial PPO controller exhibited instability and inconsistent behaviour.

### Parameter Tuning

PPO parameter tuning was attempted to improve the controller's behaviour, but tuning alone did not resolve the underlying problem.

### Reformulation

The control formulation was reconsidered, with particular attention to the state representation and reward function.
The state was refined to combine frequency deviation, previous control action, and inverter output.

The reward formulation was also refined to better represent the frequency-regulation objective and control effort.

### Final Evaluation

The refined controller demonstrated more consistent frequency-response behaviour across varying stochastic disturbance conditions.

## Evaluation

The controller was evaluated by comparing frequency responses under different operating and disturbance conditions.

The evaluation included:

- Frequency response before and after applying the RL controller
- Comparison with a conventional/fixed-rule controller
- Different load levels
- Different disturbance magnitudes
- Stochastic disturbance profiles distinct from those used during training

A training reward/convergence plot was also used to examine the learning process.

No quantitative performance metrics from the original implementation are currently available.

## Key Findings

The main findings from the project were:

- The formulation of the control problem strongly affects learning-based controller performance.
- An inadequate state representation can lead to unstable or inconsistent control behaviour.
- PPO parameter tuning alone may not resolve problems caused by an insufficient system representation.
- Including information about previous control behaviour and inverter response provided a more informative state representation.
- The refined controller showed improved response consistency under varying disturbance conditions.
- Learning-based control should be evaluated under conditions that differ from those encountered during training.

## Software

- Python
- MATLAB
- Simulink
- TensorFlow

## Reproducibility Note

The original source code and development environment are no longer available.

This repository therefore distinguishes between:

1. Historical reconstruction — documentation of the original project based on available records and recollection.
2. Reimplementation — a new implementation of the documented methodology.

Historical details that could not be verified are explicitly identified rather than inferred.

Any future implementation should therefore be regarded as a reimplementation of the research workflow, not recovery of the original source code.

## Project Status

| Component | Status |
|---|---|
| Historical project documentation | Completed |
| Original source code | Unavailable |
| System-model reconstruction | In progress |
| RL formulation reconstruction | In progress |
| Reimplementation | Planned |
| Experimental reproduction | Planned |

## Repository Structure

rl-grid-frequency-stabilisation/
├── README.md
├── docs/
│   ├── project_background.md
│   ├── system_model.md
│   ├── control_architecture.md
│   ├── rl_formulation.md
│   ├── training_workflow.md
│   ├── evaluation_methodology.md
│   └── lessons_learned.md
├── historical_reconstruction/
│   ├── known_details.md
│   └── unknown_details.md
├── reimplementation/
│   ├── environment/
│   ├── agent/
│   ├── models/
│   ├── training/
│   └── evaluation/
├── experiments/
├── results/
└── figures/