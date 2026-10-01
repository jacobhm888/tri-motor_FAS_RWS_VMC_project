# Validation and acceptance plan

Acceptance thresholds must be fixed by function owners before evaluation. Below are measurable gates and proposed experiments, not a claim that the system has passed them.

| Stage | Tests | Evidence / exit gate |
| --- | --- | --- |
| Data | Timestamp drift, missing data, calibration, label quality, tire/load coverage | Audited dataset and independent vehicle/trip split |
| Offline | Baseline vs FP32 vs compiled INT8; normal, limit, fault and unknown cases | RMSE plus peak/directional errors, confidence calibration, coverage and abstention |
| MIL with AVL VSM | CRC R40/50/60, step steering, slalom, split-mu, mu transition, bumps | Closed-loop KPI comparison using identical scenarios and bounded perturbations |
| SIL | ECU-equivalent preprocessing, schema, saturation, numeric limits | Golden-vector equivalence and documented quantization tolerance |
| HIL/target | Actual S32K5 runtime, DMA/network contention, memory peaks, task jitter | Approved worst-case budget, no control deadline degradation |
| Fault injection | Stale/gapped/reordered input, stuck sensor, NaN/Inf, OOD, runtime timeout | Correct rejection, same-cycle baseline selection and validated transition |
| Update recovery | Corrupt/unsigned payload, mismatched tuple, power loss, replay, rollback | No partial activation; deterministic known-good or AI-disabled boot |
| Vehicle shadow | Instrumented loads/INS, surfaces, modes, temperatures, tires, payloads | Held-out benefit and characterized failure/unknown cases |
| Limited fusion | Approved bounds; repeated A/B tests | No unacceptable stability/comfort regression; safety owner approval |
| Canary | Version-aware diagnostics and KPI drift | Reviewed cohort expansion or rollback decision |

## Measurements by function

Mu: estimation error where identifiable, optimistic-error tail, transition detection delay, unknown/low-excitation abstention and false high-mu classification. Do not label a gentle-driving force utilization estimate as true peak friction.

Fz: per-wheel RMSE/peak error, dynamic force/moment consistency, latency and transient accuracy. Use WFT/reference load measurements with synchronized mounting/coordinate transforms.

Vy/beta: INS-reference error with alignment uncertainty, phase lag, convergence after resets, limit-handling error and observer recovery. Fix Vx thresholds and sign conventions.

Roughness: precision/recall by event and road, severity calibration, false impact rate, road-band accuracy, body acceleration RMS and wheel-hop/contact metrics after permitted gain scheduling. Compare reactive and front-to-rear transfer separately.

Maintenance: false alarms per operating hour, detection lead time and confirmed fault sensitivity. Do not claim remaining useful life without sufficiently representative lifetime data.

## Generalization and statistics

Keep entire trips, vehicles and road campaigns out of training; overlapping windows must not straddle splits. Evaluate unseen tires, pressure, wear, temperatures, loads, sensor variants, bank/slope and strong maneuvers. Report confidence intervals across independent runs, not millions of correlated windows. Track uncertainty of labels and separate synthetic from measured performance.

## Runtime worksheet

Record exact part/board, silicon revision, clock, core/NPU mapping, SDK/compiler, operator placement, model size, scratch/activation/history RAM, stack, transfer/queue/inference/supervisor times, end-to-end age, WCET stress conditions and missed-deadline counters. VDK results are an early gate; target ECU measurements remain required.

## Proposed program sequence

Phase 0 (indicative 4–6 weeks): interfaces, recorder, baseline, tiny model compile and target benchmark. Phase 1 (8–12 weeks): surface/roughness and Fz shadow PoCs with ground truth. Phase 2 (8–12 weeks): independently gated mu/Vy experiments and closed-loop fusion assessment. Phase 3: approved update/canary process and broader functions. These are planning estimates conditional on hardware/tooling/data availability.

The current draft has only documentation/link/JSON checks. No trained-model evaluation, MIL, SIL, HIL, target profiling or vehicle validation has been executed for this package.
