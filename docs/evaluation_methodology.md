# Evaluation Methodology

## Overview

The trained reinforcement learning controller was evaluated to determine whether it could provide consistent frequency regulation under changing operating conditions and stochastic disturbances.

Evaluation focused primarily on frequency-response behaviour and comparison with conventional/fixed-rule control.

The original project did not retain a complete set of quantitative performance metrics, so this document records the evaluation methodology without introducing unsupported numerical results.

---

## Evaluation Objectives

The evaluation addressed four main questions:

1. Can the RL controller reduce frequency deviations?
2. Does the controller maintain consistent behaviour under different disturbance conditions?
3. Can the trained policy generalise to stochastic profiles different from those used during training?
4. How does the RL controller compare with conventional/fixed-rule control?

---

## Evaluation Conditions

The controller was tested under varying operating conditions, including:

- different load levels;
- different disturbance magnitudes;
- stochastic time-varying load profiles;
- profiles distinct from those used during training.

The use of different stochastic profiles was intended to assess the adaptability of the learned policy rather than simply its ability to reproduce behaviour on previously encountered disturbance sequences.

---

## Training and Test Separation

The training and evaluation disturbance profiles were not identical.

The PPO policy was trained using one set of stochastic conditions and subsequently evaluated using different stochastic profiles.

Conceptually:

```text
Training
──────────────
Stochastic profile set A
        │
        ▼
     PPO training
        │
        ▼
   Trained policy


Evaluation
──────────────
Stochastic profile set B
        │
        ▼
   Trained policy
        │
        ▼
Frequency response
```
This provided a basic assessment of generalisation to unseen disturbance sequences.

## Frequency-Response Evaluation
Frequency response was the primary observable used to assess controller behaviour.
For each test condition, the system response was examined with and without the learning-based controller.
The evaluation considered the extent to which the RL controller could maintain frequency closer to the desired operating condition following disturbances.
The comparison was primarily based on the observed frequency-response curves.

## Conventional Controller Comparison
The RL controller was compared against a conventional/fixed-rule control strategy.
The purpose of this comparison was to determine whether the adaptive learning-based controller could provide more consistent regulation when operating conditions changed.
The comparison considered the frequency response under the same or comparable disturbance conditions.
The exact implementation and parameter values of the historical conventional controller are unavailable.

## Disturbance Magnitude
Multiple disturbance magnitudes were considered during evaluation.
This allowed the controller to be examined under disturbances of different severity rather than under a single test condition.
The exact numerical disturbance magnitudes from the original experiments are not available.

## Load-Level Evaluation
The controller was also evaluated at different load levels.
This was important because the project was concerned with controller behaviour under changing operating conditions.
The evaluation therefore considered whether the learned policy remained effective when the system operated at different levels of load demand.
The exact historical load values are unavailable.

## Before-and-After Comparison
One component of the evaluation compared frequency response before and after applying the RL controller.
Conceptually:
```text
                 Disturbance
                     │
                     ▼
              ┌─────────────┐
              │   Microgrid │
              └──────┬──────┘
                     │
          ┌──────────┴──────────┐
          │                     │
          ▼                     ▼
    Without RL              With RL
          │                     │
          ▼                     ▼
 Frequency response       Frequency response
          │                     │
          └──────────┬──────────┘
                     ▼
                  Compare
```
The comparison was used to examine the effect of the learned controller on frequency behaviour.

## Training-Reward Analysis
A training reward/convergence plot was produced during the project.
The reward history provided an indication of the learning process and was used to examine whether the PPO agent was improving during training.
The original numerical reward history is not currently available.

## Performance Criteria
The project primarily examined:
- frequency-response consistency;
- reduction of frequency deviation;
- behaviour across different disturbance conditions;
- adaptability to changing operating conditions;
- control effort;
- comparison with fixed-rule control.

The project did not retain verified numerical values for standard metrics such as:
- settling time;
- maximum frequency deviation;
- steady-state error;
- integral absolute error (IAE);
- integral squared error (ISE);
- overshoot;
- control-energy measures.
These metrics should therefore not be reported as historical results.
They may be introduced during a future reimplementation as part of a more rigorous quantitative evaluation.

## Observed Outcome
The refined RL controller demonstrated improved response consistency relative to fixed-rule control approaches across the tested varying conditions.
The improvement followed the reformulation of the RL problem, particularly the refinement of the state representation and reward formulation.
The project therefore suggested that controller performance was influenced strongly by how the physical control problem was represented to the learning algorithm.

## Recommended Reimplementation Evaluation
A future reproduction should use a structured evaluation matrix.
```text
                 Disturbance Magnitude
                Low      Medium      High
              ┌────────┬───────────┬────────┐
Load Level 1  │   ✓    │     ✓     │   ✓    │
Load Level 2  │   ✓    │     ✓     │   ✓    │
Load Level 3  │   ✓    │     ✓     │   ✓    │
              └────────┴───────────┴────────┘
```
Each condition should be evaluated using:
- conventional/fixed-rule control;
- PPO control;
- identical disturbance conditions for fair comparison.
Where possible, multiple stochastic realisations should be used for each operating condition.

## Recommended Quantitative Metrics
Although these metrics were not retained from the original project, they would strengthen a future reproduction.

### Maximum Frequency Deviation
Measures the largest departure from nominal frequency following a disturbance.

### Settling Time
Measures how long the system takes to return and remain within a specified frequency-error band.

### Steady-State Error
Measures the residual frequency deviation after the transient response.

### Integral Absolute Error
Measures the accumulated magnitude of frequency deviation over the evaluation period.

### Control Effort
Measures the magnitude or energy of the control action required from the battery inverter.
These metrics should be defined before the reimplementation experiments are conducted.

## Fair Comparison
For a valid comparison between controllers:
1. Use the same microgrid model.
2. Apply the same disturbance realisations.
3. Use the same initial operating conditions.
4. Evaluate over the same simulation duration.
5. Record the same performance metrics.
6. Repeat stochastic tests using multiple random seeds where appropriate.
The only intended difference should be the controller being evaluated.

## Limitations
The historical evaluation is limited by the loss of the original source code, simulation model, numerical results, and detailed experiment records.
Consequently, this documentation does not claim:
- exact numerical performance improvements;
- exact disturbance magnitudes;
- exact load levels;
- exact training/test sample counts;
- exact evaluation duration;
- exact conventional-controller parameters.
These values should be recovered from surviving project files if they become available or established explicitly during reimplementation.

## Evaluation Summary
The historical evaluation can be summarised as:
```text
Different load levels
        +
Different disturbance magnitudes
        +
Stochastic disturbance profiles
        │
        ▼
   Trained PPO policy
        │
        ▼
 Frequency-response analysis
        │
        ├── RL controller
        │
        └── Conventional control
        │
        ▼
Compare stability, consistency,
adaptability and control effort
```
The principal conclusion was that the refined learning-based controller produced more consistent frequency-regulation behaviour across the tested varying conditions.
