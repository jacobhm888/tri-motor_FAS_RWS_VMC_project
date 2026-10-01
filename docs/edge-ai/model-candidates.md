# Practical edge-model candidates

All rates and sizes below are proposed experiments, not measured S32K5 capabilities. Compare each model against the current observer and a simple statistical/lookup baseline. Use the smallest model that improves held-out vehicle performance.

| Candidate | Inputs and output | First model / experiment | Main limitation | Priority |
| --- | --- | --- | --- | --- |
| Event novelty | Observer residuals, interventions, validity; event score | Threshold baseline, then small autoencoder; 10–50 Hz | Anomaly is not a diagnosed fault; bound false uploads | Foundation |
| Surface class | Wheel speeds, torque, IMU, steering; configured classes plus unknown | Feature MLP then causal 1D CNN/TCN; 25–100 Hz | Weak excitation, tire and load changes; class is not friction | First PoC |
| Roughness/impact | Body/wheel Z acceleration, heights, speed; severity and frequency bands | DSP baseline then causal TCN; 100–200 Hz input | Sensor placement, aliasing, speed dependence | First PoC |
| Fz residual | Physics Fz, IMU, heights, suspension state; four load residuals | MLP then short TCN; 50–100 Hz | Need WFT/reference loads; preserve dynamic force balance | Second PoC |
| Friction residual | Slip, forces, IMU, torque, pressure, observer state; delta-mu and uncertainty | MLP/TCN; 50–100 Hz | Available peak friction often unobservable in gentle driving | Later, shadow first |
| Beta/Vy residual | Steering, speed, yaw, acceleration, force/model residuals; delta-Vy | MLP/TCN; 50–100 Hz | INS truth, time alignment and stability near limits | Later, shadow first |
| Cf/Cr correction | Lateral excitation and observer residuals; bounded axle stiffness multipliers | Gated RLS baseline versus MLP; 1–10 Hz update | Confounding with mass, mu and tire saturation | Later |
| Component health | Commands, response, current, temperature; anomaly score | Statistics/autoencoder; 1–10 Hz | Sparse verified faults; RUL needs lifetime labels | Parallel advisory |

Use a GRU only if recurrent state yields a measured advantage and target operator support is verified. Reset hidden state after gaps, version changes and startup. Avoid bidirectional models and centered windows because they use unavailable future data.

## Sizing an experiment

Start with 10k–100k parameters for an MLP and 50k–300k for a small TCN. These are search ranges. INT8 weights use roughly one byte per parameter before compiler overhead; activation RAM, histories, DMA buffers and alignment can dominate. A 32-channel, 100-sample history stored as float32 alone uses 12,800 bytes. Record actual compiled artifacts and peak runtime memory.

Quantify feature cost, transfer time, queueing, inference, output checks and control delivery together. At a 10 ms inference period, an initial integration target might allocate 2 ms to the complete AI path, but the scheduler owner must approve a budget based on measured worst-case interference. A 5 ms VMC controller need not run AI every cycle; use timestamped outputs with an explicit freshness limit.

A temporal window represents history, not automatically equal delay. Measure causal filter group delay, output lag and transient performance. NPU use is conditional on the exact device, SDK, operator coverage, partition and memory budget. Unsupported operators or CPU spillover require remeasurement.

## Observability decisions

Surface classes provide a contextual prior. Do not equate asphalt with high mu or infer peak mu from class confidence alone. Use an excitation/observability score and uncertainty; retain a conservative physics estimate when evidence is insufficient. Learn Cf/Cr in sufficiently excited unsaturated conditions; do not update all Magic Formula B/C/D/E parameters from ordinary CAN data without an identifiability study.

Estimate either Vy or beta as the independent correction and derive the other consistently under the documented coordinate convention. At low speed use the existing observer's validity policy rather than dividing by a near-zero Vx.
