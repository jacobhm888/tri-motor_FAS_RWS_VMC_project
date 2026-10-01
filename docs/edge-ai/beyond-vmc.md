# Examples beyond VMC

Reuse preprocessing, model registry, confidence handling, signed updates and event capture. Each domain retains its own protection and owner-approved authority.

| Application | Candidate model and inputs | Practical output | Independent boundary / evidence |
| --- | --- | --- | --- |
| Pump/compressor maintenance | Small autoencoder; current, speed, temperature, command/response | Anomaly trend and service flag | Keep overload protections; bench faults and confirmed repair labels |
| Motor/inverter efficiency | MLP residual; speed, torque, DC power, temperature | Corrected loss prediction for deterministic allocation | Torque/power/thermal limits remain independent; dyno truth |
| Battery/thermal prediction | MLP/TCN; BMS-authorized SOC, current, temperatures | Near-term temperature or energy forecast | No override of BMS SOP/contactors; pack bench and vehicle validation |
| HVAC efficiency | MLP; cabin/ambient temperature, occupancy proxy, compressor power | Bounded setpoint or efficiency suggestion | Defogging and visibility priorities; climate chamber tests |
| Driver preference | Small classifier; pedal/steering statistics and chosen modes | Select approved comfort/sport calibration | Opt-in/reset; no expansion of stability envelope |
| Manufacturing end-of-line | Classifier; controlled actuator-response traces | Test anomaly and retest suggestion | Existing pass/fail checks retained; known-good/fault parts |
| Zonal sensor health | Statistics then autoencoder; cross-sensor innovations | Suspect-sensor flag and logging trigger | Existing DTC and degradation authority; injected bias/dropout tests |

An advisory maintenance model is a useful early business demonstration because it can remain outside control. Energy optimization needs an A/B protocol controlling route, temperature, payload and driver; lower predicted losses do not establish real vehicle energy savings. Fleet models must separate hardware/tire/software variants to prevent domain mixing.
