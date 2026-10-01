# VMC Edge AI proposal

**Draft for review · 2026-10-01 · no production release**

Enhance existing vehicle observers with small edge models and reviewed cloud updates. Keep deterministic VMC controllers, actuator arbitration and safety limits authoritative. Start with event capture and road recognition in shadow mode; progress to bounded state-estimation corrections only when independently measured benefits justify the integration.

## Read the package

| Document | Purpose |
| --- | --- |
| [Model candidates](model-candidates.md) | Practical models, observability, priorities and resource sizing |
| [Architecture](architecture.md) | Edge SWCs, cloud learning and real-time isolation |
| [Interfaces](interfaces.md) | Logical contracts, timing, validity and actuator boundaries |
| [Safety bounds](safety-bounds.md) | Confidence gates, bounded fusion and fallback |
| [Update lifecycle](update-lifecycle.md) | Signed releases, compatibility, activation and rollback |
| [Validation](validation.md) | Data, MIL/SIL/HIL/vehicle tests and release evidence |
| [Beyond VMC](beyond-vmc.md) | Maintenance, energy, thermal and other applications |
| [Examples](examples/README.md) | Synthetic correction and update scenarios |
| [Sources and assumptions](sources.md) | Hardware evidence and unresolved decisions |

## Proposed first delivery

1. Establish synchronized feature capture, a deterministic fallback and target measurements.
2. Compare a small MLP and a causal TCN for surface/roughness recognition using chassis signals. No video input is assumed.
3. Capture novel events and validate wheel-load residuals against instrumented measurements.
4. Evaluate friction and sideslip residuals in shadow mode; introduce limited fusion only after safety review.

Cloud training uses fleet, test and AVL VSM data. A teacher may be a larger temporal model or ensemble; an LLM is optional for engineering analysis and has no control role. Deploy a fixed reviewed student and calibration pair. Field learning creates proposed releases rather than silently changing live weights.

This package extends the [original Chinese proposal](../VMC_Edge_AI_Cloud_Edge_Collaboration_Proposal.md). Numerical examples are illustrative and must not be used as vehicle calibration. The package contains documentation and synthetic examples, with no trained model, supplier DBC, production calibration, OTA implementation or measured S32K5 performance.
