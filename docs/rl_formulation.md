# Reinforcement Learning Formulation

## Overview

The frequency regulation problem was formulated as a continuous-control reinforcement learning task.

A Proximal Policy Optimization (PPO) agent interacted with a simulated microgrid environment and generated continuous control signals for an inverter control block associated with battery energy storage.

The formulation was designed to enable the agent to respond adaptively to frequency disturbances rather than relying solely on a fixed control rule.

---

## Control Objective

The primary objective was to maintain microgrid frequency close to its nominal operating value following changes in system conditions and disturbances.

The controller was required to balance two related objectives:

1. Minimise frequency deviation.
2. Avoid unnecessarily large control actions.

This created a trade-off between frequency regulation performance and control effort.

---

## Reinforcement Learning Components

The problem was formulated using the standard reinforcement learning components:

| Component | Project formulation |
|---|---|
| Environment | MATLAB/Simulink microgrid simulation |
| Agent | PPO |
| State | Frequency deviation, previous control action, inverter output |
| Action | Continuous control signal |
| Reward | Frequency regulation performance with a penalty for excessive control effort |
| Objective | Learn an adaptive frequency-regulation policy |

---

## State Representation

### Initial Formulation

The initial state representation did not provide sufficient information about the evolving behaviour of the system.

The controller consequently exhibited unstable or inconsistent behaviour under some operating conditions.

Parameter tuning was attempted, but tuning the PPO parameters alone did not resolve the underlying problem.

### Reformulated State

The state representation was subsequently reconsidered to provide the agent with more information about the system dynamics.

The refined state combined:

- current frequency deviation;
- previous control action;
- inverter output.

Conceptually:

$$
s_t = \left[\Delta f_t,~ u_{t-1},~ y_{\mathrm{inv},t}\right]
$$

where:

- $$\(\Delta f_t\)$$ represents the frequency deviation at time \(t\);
- $$\(u_{t-1}\)$$ represents the control action from the previous time step;
- $$\(y_{\mathrm{inv},t}\)$$ represents the inverter output.

The purpose of the reformulation was to provide information about both the current frequency condition and the recent control/system response.

The key observation from the development process was that:

> The state representation wasn't capturing enough of the system dynamics for the controller to make consistent decisions.

---

## Action Space

The PPO agent operated in a continuous action space.

At each decision step, the agent generated a continuous control signal that was passed to the inverter control block.

The inverter control block provided the lower-level control implementation for the battery inverter.

The PPO agent therefore acted as a higher-level controller rather than directly controlling inverter switching.

The exact historical action limits and physical units are not available in the surviving project records.

---

## Reward Design

The reward function was designed around the frequency-regulation objective.

The controller was encouraged to:

- reduce frequency deviation;
- avoid excessive control effort.

A conceptual representation of the reward structure is:

$$
r_t = -\alpha |\Delta f_t| - \beta |u_t|
$$

where $\alpha$ and $\beta$ represent the relative weighting of frequency deviation and control effort.

**Note:** This equation represents the reconstructed structure of the reward formulation. The exact historical equation and numerical weighting coefficients are unavailable and should not be treated as recovered source-code values.

The reward design was refined alongside the state representation during the controller reformulation.

---

## Environment

The reinforcement learning environment represented the microgrid dynamics in MATLAB/Simulink.

The environment provided system observations to the PPO agent and received the agent's continuous control signal.

Conceptually, the interaction followed:

```text
        State
          │
          ▼
     ┌─────────┐
     │   PPO   │
     │  Agent  │
     └────┬────┘
          │
          │ Continuous action
          ▼
┌──────────────────────┐
│ Inverter Control     │
│ Block                │
└──────────┬───────────┘
           │
           ▼
     Battery Inverter
           │
           ▼
        Microgrid
           │
           │
           ▼
   Frequency / System
      Response
           │
           └──────────────► RL Environment
```
The exact historical mechanism used to exchange data between Python and MATLAB/Simulink is no longer available.

## Disturbance Model

The controller was exposed to stochastic, time-varying load conditions.
Rather than relying only on fixed step disturbances, the project used random/stochastic time-series variations in load demand.
The broader evaluation also considered different disturbance magnitudes and operating conditions.
The use of stochastic disturbances was intended to test whether the learned policy could maintain consistent behaviour when the system did not encounter exactly the same conditions repeatedly.

## Training and Evaluation Separation

The trained policy was evaluated using stochastic profiles distinct from those used during training.
This provided a basic test of whether the learned controller could generalise beyond the specific disturbance sequences encountered during training.
The evaluation considered multiple load levels and disturbance magnitudes.

## Controller Development Process

The RL controller was developed iteratively.

### Stage 1 — Initial RL Formulation
The microgrid frequency-regulation problem was formulated as a continuous-control RL problem and a PPO agent was trained to generate inverter control actions.
### Stage 2 — Initial Controller Behaviour
The initial controller exhibited instability and inconsistent responses.
### Stage 3 — Parameter Tuning
PPO parameters were adjusted in an attempt to improve the controller's behaviour.
This did not adequately resolve the instability.
### Stage 4 — Reformulation
The control problem itself was reconsidered.
The state representation was identified as insufficiently informative about the system dynamics. The state was refined to incorporate frequency deviation, previous control action, and inverter output.
The reward formulation was also refined to better balance frequency regulation and control effort.
### Stage 5 — Evaluation
The resulting controller was evaluated across different load levels, disturbance magnitudes, and stochastic disturbance profiles.
The refined formulation produced more consistent frequency-regulation behaviour.
