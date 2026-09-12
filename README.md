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