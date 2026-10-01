# Proposed logical interfaces

These names are new logical SWC contracts, not verified DBC/SIDL signals. Exact bus IDs, scaling, endian layout, service IDs and runnable mapping require review against the released interface database.

## Feature snapshot

| Field group | Units/convention | Quality requirements |
| --- | --- | --- |
| sample_time_us, sequence, schema_id | Monotonic ECU time; sequence wrap defined | Atomic synchronized snapshot |
| Vx, Vy_physics, yaw_rate, ax, ay | m/s, m/s, rad/s, m/s²; vehicle axes specified | Age, source validity and observer confidence |
| wheel_speed[4], wheel_load_physics[4] | rad/s, N; order FL FR RL RR | Individual validity, sensor/estimator provenance |
| front/rear steering | rad at road wheel; positive sign documented | EPS/RWS status and valid flags |
| motor torque, estimated brake torque | N·m; applied/actual versus requested identified | Torque latency and uncertainty |
| height/body_z/wheel_z[4] | m, m/s², m/s² | Alignment, mounting conversion and anti-alias filtering |
| drive mode, ABS/ESC/TCS state, faults | Enumerations versioned in schema | Gate policy per feature/model |
| physics_mu, slope, mass estimate | dimensionless, rad, kg | Confidence and update age |

Associate each field with valid, age and source flags. Use a defined missing-data policy; do not impute invalid safety-critical inputs silently. Freeze the feature order, normalization, window length, sample period and coordinate transforms in the release schema/hash. Training preprocessing must match ECU preprocessing exactly.

## Inference result

| Field | Type/units | Consumer |
| --- | --- | --- |
| model_id, model_version, calibration_id, schema_hash | Versioned identifiers | Supervisor, recorder, diagnostics |
| input_time_us, completion_time_us, sequence | uint64/uint32 logical fields | Freshness and association checks |
| residual_kind, residual[] | Enum; SI values such as delta-mu or delta-Vy | Fusion only |
| class_probabilities[], class_map_id | [0,1], sum checked | Context scheduling |
| confidence, ood_score, observability | [0,1] with explicitly defined semantics | Supervisor |
| valid, status, execution_time_us | Boolean, enum, time | Deadline and fault monitoring |

A softmax probability is not a guaranteed probability of correctness. Calibrate confidence on held-out data and retain independent OOD, input-validity and physical checks. Record rejection reasons: stale, invalid signal, OOD, low excitation, out of envelope, incompatible version, deadline, nonfinite or runtime fault.

## Existing control boundary

Fusion outputs pass to existing observers/control SWCs through reviewed RTE contracts. AI has no actuator-write interface. Existing drive/brake limits, ESC/IPB arbitration, RWS/eTV coordination and CDC/HAS/ASU gates remain enforced after fusion. Four-wheel brake requests remain restricted to the existing All Terrain allowance. CDC PWM, MPU commands and ASU Up/Down Request plus Rate Level remain deterministic controller outputs.

Without camera video, roughness estimation is reactive at the front axle. A front-to-rear transfer may offer approximately wheelbase/Vx lead time, reduced by processing delay and path mismatch; gate on speed, steering and wheel-path agreement. Do not describe it as guaranteed preview ahead of the front axle.

## Release interface

The bundle carries model/calibration IDs, feature-schema hash, exact compatible hardware/runtime/software IDs, payload hashes, signature metadata, monotonic release counter, validity policy, rollback policy and evidence references. Safety-envelope changes require a controller/safety release review; a model package cannot expand its own permitted authority. The [sample manifest](examples/release-manifest.json) is deliberately unsigned and non-installable.
