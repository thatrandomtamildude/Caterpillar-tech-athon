# Caterpillar-tech-athon
Intelligent adaptive PI tuning studied and extracted from TD3 which is a successor of DDPG algorithm , It uses 2 quality service instead of one as contrast from DDPG ! , 
TD3-Based Adaptive PI Controller for Engine Speed Control
Overview

This project implements a Twin Delayed Deep Deterministic Policy Gradient (TD3) based adaptive control strategy for engine speed regulation.

Instead of replacing the conventional PI controller, the reinforcement learning agent continuously adjusts the PI gains through correction factors, allowing the controller to adapt to changing operating conditions and disturbances while maintaining stable performance.

The objective is to achieve and maintain a target engine speed of 900 RPM with:

Fast tracking response
Minimal overshoot
Reduced steady-state error
Improved disturbance rejection
Smooth controller actions
Control Architecture

The control system consists of:

Fixed PI Controller

The base PI controller uses predefined gains:

K
p,fixed
	​

K
i,fixed
	​

TD3 Agent

The TD3 agent generates two correction factors:

α (Proportional gain scaling factor)
β (Integral gain scaling factor)

The final controller gains are computed as:

K
p,final
	​

=K
p,fixed
	​

×α
K
i,final
	​

=K
i,fixed
	​

×β

This enables real-time adaptive tuning without sacrificing the stability of the classical PI framework.

State Space

The agent observes a 5-dimensional state vector:

State	Description
e(t)	Speed tracking error
de(t)	Change in error
A	Disturbance parameter
α_prev	Previous α value
β_prev	Previous β value
Action Space

The actor network outputs:

Action	Description
α	Proportional gain correction factor
β	Integral gain correction factor

These values are used to adapt the PI controller during operation.

Reward Function

The reward is designed to encourage:

Accurate RPM tracking
Low overshoot
Smooth gain adaptation
Fast settling
reward = ...
    -1.0*abs(error) ...
    -0.1*error^2 ...
    -4.0*max(0,rpm-900) ...
    -0.1*abs(alpha-alpha_prev) ...
    -0.1*abs(beta-beta_prev);
Reward Components
Term	Purpose
abs(error)	Penalizes tracking error
error²	Strong penalty for large deviations
max(0,rpm-900)	Penalizes overshoot above 900 RPM
abs(α−α_prev)	Encourages smooth gain updates
abs(β−β_prev)	Prevents abrupt controller changes
Disturbance Modeling

A varying disturbance is introduced into the system through:

Signal Builder
      ↓
Gain (20)
      ↓
+15
      ↓
Parameter A

Disturbance equation:

A=20×Signal+15

This allows the agent to learn robust control under changing operating conditions.

TD3 Features

The implementation includes:

Twin Critic Networks
Delayed Policy Updates
Experience Replay Buffer
Target Networks
Soft Target Updates
Continuous Action Space Control

These mechanisms improve stability and reduce value overestimation compared to standard DDPG.

Training Configuration
Parameter	Value
Algorithm	TD3
Max Episodes	100
Max Steps/Episode	1000
Sample Time	1 s
Observation Dimension	5
Action Dimension	2
Project Workflow
Engine model generates RPM response.
Error is computed relative to the 900 RPM reference.
TD3 agent observes system states.
Actor outputs α and β.
PI gains are updated.
Engine receives control input.
Reward is calculated.
Experience is stored and used for TD3 learning.
Expected Outcomes

After training, the TD3-based adaptive PI controller should:

Reach 900 RPM rapidly
Reduce overshoot significantly
Handle disturbances effectively
Maintain stable operation
Adapt controller gains automatically
Technologies Used
MATLAB
Simulink
Reinforcement Learning Toolbox
TD3 Algorithm
Control Systems Engineering
Adaptive PI Control
Author

Yashwanth
B.E. Electrical and Electronics Engineering
TD3-Based Adaptive Engine Speed Control Research Project
