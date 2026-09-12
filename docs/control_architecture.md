# Control Architecture

## Overview

The project used a hierarchical control structure in which a reinforcement learning agent provided a higher-level control signal to an inverter control block associated with battery energy storage.

The PPO agent was responsible for learning an adaptive control policy, while the lower-level inverter control block handled the implementation of the control command.

The overall objective was to regulate microgrid frequency under changing operating conditions and stochastic disturbances.

---

## Control Structure

The conceptual control architecture was:

```text
                    Microgrid
                       │
          ┌────────────┴────────────┐
          │                         │
   Conventional               Battery Energy
    Generation                   Storage
                                  │
                                  ▼
                           Battery Inverter
                                  │
                                  ▼
                        Inverter Control Block
                                  ▲
                                  │
                       Continuous Control
                             Signal
                                  │
                                  ▲
                           PPO Agent
                                  ▲
                                  │
                           RL Environment
                                  ▲
                                  │
                      System Measurements
```
This represents the functional architecture reconstructed from the available project information. It does not claim to reproduce the exact historical Simulink block diagram.

## PPO Supervisory Controller

The PPO agent operated as a supervisory controller.

At each decision step, the agent received an observation representing the current state of the system and generated a continuous control action.

The action was passed to the inverter control block, which provided the lower-level implementation required to control the battery inverter.

The PPO agent therefore did not directly perform inverter switching.

## Control Loop

The control process can be represented as:

```text
1. Measure system response
        ↓
2. Construct RL state
        ↓
3. PPO selects continuous action
        ↓
4. Send action to inverter control block
        ↓
5. Battery inverter responds
        ↓
6. Microgrid dynamics evolve
        ↓
7. Measure new system response
        ↓
8. Calculate reward
        ↓
9. Repeat
```
This process was repeated throughout the simulation during training and evaluation.

## RL State

The refined state representation consisted of:
```text
State =
[
    Frequency deviation,
    Previous control action,
    Inverter output
]
```
These variables provided information about:
- the current frequency condition;
- the controller's recent action;
- the current inverter response.

The state was refined after the initial formulation was found to be insufficient for capturing enough of the system dynamics to support consistent control decisions.

## Control Action

The PPO agent generated a continuous control signal.

The signal was passed to the inverter control block and used to influence the battery inverter's response.

The exact physical interpretation, units, scaling, and historical action limits are not currently available.

These parameters should therefore be established during reimplementation rather than inferred.

## Lower-Level Inverter Control

The inverter control block formed the interface between the PPO supervisory controller and the battery inverter.

Its role was to receive the higher-level continuous control command and implement the corresponding inverter behaviour.

The exact historical control algorithm, controller gains, modulation strategy, and switching implementation are unavailable.

These details are outside the confirmed scope of the historical reconstruction.

## Feedback

Feedback from the simulated system was used to construct the RL state.

The most important feedback variables identified in the reconstruction were:

```text
Frequency deviation
Previous control action
Inverter output
```
The inclusion of the previous control action provided information about recent controller behaviour, while inverter output provided information about the response of the controllable resource.

Together with frequency deviation, these signals formed the refined representation used by the PPO controller.

## Training Architecture

During training, the interaction between the Python-based PPO agent and MATLAB/Simulink environment followed the conceptual structure:
```text
┌─────────────────────┐
│ Python              │
│                     │
│ PPO Agent           │
│                     │
│ State → Action      │
└──────────┬──────────┘
           │
           │ Continuous action
           ▼
┌─────────────────────┐
│ MATLAB / Simulink   │
│                     │
│ Inverter Control    │
│        ↓            │
│ Battery Inverter    │
│        ↓            │
│ Microgrid Model     │
└──────────┬──────────┘
           │
           │ System response
           ▼
┌─────────────────────┐
│ RL Environment      │
│                     │
│ State + Reward      │
└──────────┬──────────┘
           │
           └──────────────► PPO Agent
```
The exact Python-MATLAB communication mechanism used in the original implementation is not available.

## Development of the Architecture

The architecture evolved during development.

### Initial Controller

The initial PPO controller produced unstable or inconsistent behaviour.

### Parameter Tuning

PPO parameters were adjusted, but the observed instability persisted.

### State Reformulation

The problem was subsequently traced to the control formulation rather than being treated solely as an optimisation or hyperparameter problem.

The state representation was revised to provide more information about the system's evolving behaviour.

### Refined Controller

The refined state incorporated frequency deviation, previous control action, and inverter output.

The reward formulation was also refined to balance frequency regulation and control effort.

The resulting controller demonstrated improved consistency across the tested disturbance conditions.

## Conventional Control Comparison

The RL controller was compared against a conventional/fixed-rule control strategy.

The comparison focused on frequency-response behaviour under varying operating conditions and disturbance magnitudes.

The purpose was to determine whether the learning-based controller could provide more consistent adaptive regulation than a fixed control strategy.

The exact implementation and parameter values of the historical conventional controller are not currently available.
