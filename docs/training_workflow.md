# Training Workflow

## Overview

The reinforcement learning controller was trained using Proximal Policy Optimization (PPO) in Python.

The microgrid dynamics were represented in MATLAB/Simulink, which provided the simulated environment through which the controller interacted with the system.

The training process was designed to expose the agent to changing operating conditions and stochastic disturbances so that it could learn an adaptive frequency-regulation policy.

---

## Training Architecture

The conceptual training workflow was:

```text
             MATLAB / Simulink
             Microgrid Model
                    │
                    ▼
             System Response
                    │
                    ▼
              RL Environment
                    │
                    │ State
                    ▼
             Python / PPO Agent
                    │
                    │ Continuous Action
                    ▼
          Inverter Control Block
                    │
                    ▼
             Battery Inverter
                    │
                    └──────────────► Microgrid
```

## Training Process
At each interaction step, the following sequence was conceptually followed:
1. The microgrid simulation evolved according to the current operating conditions and disturbances.
2. Relevant system measurements were obtained.
3. The RL environment constructed the agent's state.
4. The PPO agent selected a continuous control action.
5. The action was passed to the inverter control block.
6. The battery inverter responded to the control command.
7. The microgrid dynamics evolved.
8. The resulting frequency response was observed.
9. A reward was calculated based on frequency regulation and control effort.
10. The resulting state, action, and reward contributed to PPO training.
11. This interaction was repeated over the training process.

## State During Training
The refined state representation consisted of:
```text
[frequency deviation,
 previous control action,
 inverter output]
```
The state was designed to provide information about the current frequency condition and the recent control/system response.
The refinement followed instability observed with the initial formulation.

## Action During Training
The PPO agent operated in a continuous action space.
The action represented a continuous control signal sent to the inverter control block.
The exact historical action range, scaling, and physical units are unavailable.

## Reward During Training
The reward encouraged the controller to minimise frequency deviation while avoiding excessive control effort.
Conceptually:
```text
Reward
   │
   ├── Reduce frequency deviation
   │
   └── Penalise excessive control action
```
The exact historical reward equation and weighting coefficients are unavailable.

## Stochastic Training Conditions
Training incorporated stochastic, time-varying load disturbances.
The use of random load profiles exposed the agent to varying system conditions rather than a single deterministic disturbance sequence.
The broader project also considered changes in renewable generation as part of the changing operating conditions.
The exact disturbance-generation procedure and numerical parameters are unavailable.

## Training and Evaluation Profiles
The trained policy was evaluated using stochastic disturbance profiles distinct from those used during training.
This separation provided a basic test of the policy's ability to generalise beyond the specific disturbance sequences encountered during training.
The evaluation also considered different load levels and disturbance magnitudes.

## PPO Training
PPO was selected as the reinforcement learning algorithm for the continuous-control problem.
The original implementation included monitoring of training reward/convergence behaviour.
A training reward plot was produced as part of the project evaluation.
The following historical implementation details are no longer available:
- Number of training episodes
- Training duration
- Learning rate
- Discount factor
- GAE parameters
- PPO clipping parameter
- Entropy coefficient
- Batch size
- Number of optimisation epochs
- Neural-network architecture
- Optimiser configuration
- Exact PPO implementation/library
These values should not be inferred.

## Initial Training Behaviour
The initial controller exhibited unstable and inconsistent behaviour.
Parameter tuning was attempted to improve the controller's performance.
However, the instability could not be adequately resolved through parameter tuning alone.
This led to a reconsideration of the underlying control formulation.

## Reformulation
The state representation was identified as a key limitation.
The initial state was not capturing enough of the system dynamics for the controller to make consistent decisions.
The state representation was therefore refined to include:
```text
Frequency deviation
Previous control action
Inverter output
```
The reward formulation was also refined to better balance frequency regulation and control effort.
The controller was subsequently retrained using the revised formulation.

## Training Outcome
Following the reformulation, the controller exhibited more consistent behaviour across the tested stochastic disturbance conditions.
The improvement was therefore attributed not simply to PPO parameter adjustment, but to improving the information provided to the controller and better aligning the reward with the control objective.

## Training Workflow Summary
```text
Define microgrid frequency-regulation problem
                    │
                    ▼
        Build RL environment
                    │
                    ▼
        Define initial state
                    │
                    ▼
          Define reward
                    │
                    ▼
            Train PPO
                    │
                    ▼
       Observe instability
                    │
                    ▼
        Attempt parameter tuning
                    │
                    ▼
      Reconsider state formulation
                    │
                    ▼
     Refine state representation
                    │
                    ▼
        Refine reward function
                    │
                    ▼
           Retrain PPO
                    │
                    ▼
 Evaluate under stochastic conditions
                    │
                    ▼
      Compare with conventional
             control
```

## Reimplementation Workflow
A future implementation should reproduce the development process rather than immediately implementing only the final formulation.
Recommended experimental sequence:
```text
Experiment 01
Initial state + reward formulation
        │
        ▼
Experiment 02
Parameter tuning
        │
        ▼
Experiment 03
State/reward reformulation
        │
        ▼
Experiment 04
Final training and evaluation
```
This structure allows the effect of the control-problem formulation to be investigated explicitly.

## Historical Reproducibility Note
The original training environment and source code are unavailable.
The documented workflow represents the reconstructed methodology based on the available project information.
Any PPO hyperparameters, network architecture, software versions, communication mechanisms, or other implementation details introduced during reimplementation must be clearly identified as newly selected values rather than historical values.
