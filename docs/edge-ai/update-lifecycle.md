# Cloud-to-edge update lifecycle

## What can change

| Release kind | Contents | Required impact review |
| --- | --- | --- |
| Calibration | Bias, confidence calibration, approved gain-table selection, normalization | Normalization changes alter effective model behavior; validate with the exact model |
| Model | Topology, weights, quantization, compiled binary, operator/runtime requirements | Full compatibility, timing, numerical and closed-loop evidence |
| Safety policy | Authority limits, operating envelope, freshness/fault policies | Separate controller and safety review; never self-authorized by AI |

Do not assume calibration-only releases are safe or exempt from regression. A normalization or confidence threshold change can be as consequential as new weights. Store and activate an immutable compatible model/calibration/schema tuple.

## Release steps

1. Trigger and upload authorized events through the vehicle gateway. Retain input validity, timestamps, software/model/calibration versions and collection conditions.
2. Curate labels and splits by vehicle, trip, tire and surface. Track simulation provenance and label uncertainty. Train teacher/student and compare to fixed baselines.
3. Quantize and compile with pinned tooling; validate the deployed artifact rather than only the training graph. Build a reproducible manifest and evidence index.
4. Obtain control, safety, cybersecurity and release-owner approval. Sign metadata and payload hashes with managed keys. Register the release immutably.
5. Download to an inactive slot. Check signature/trust chain, payload integrity, target compatibility, release authorization, storage, power and anti-replay policy. CRC is not authentication.
6. Activate atomically only in an approved parked/inhibited condition. Never replace weights mid-inference or during an active maneuver. Reset histories and recurrent state, run self-tests/golden vectors and start in shadow.
7. Roll out to an approved canary cohort; monitor rejection rate, deadline misses, observer residuals, control KPIs and diagnostic changes. Expand only after review.
8. On failure, retain or restore the known-good tuple. If no verified tuple is available, disable AI and retain baseline control. Log the exact reason and version.

## Transaction and recovery

Use inactive-slot write → verification → pending activation → self-test → confirmed activation. The persistent record must survive power loss at any step and identify a complete tuple. Boot must never select a partially written artifact. Record active and previous digests plus activation status in a protected journal.

Anti-rollback and emergency rollback need a coherent policy: a higher authorized release counter may explicitly reference an older approved model digest. Avoid blindly decrementing a security counter. Define key rotation/revocation, emergency recovery, update expiry handling and vehicle-offline behavior with the OTA/security owner.

Cloud outage leaves the current approved artifact running. Monitoring cannot be the only safety mechanism. Automatic local rejection disables a bad contribution; fleet-wide replacement requires the authorized release process. No uncontrolled on-vehicle weight training is proposed.

## Required evidence per release

Dataset/label versions; train/evaluation split definition; preprocessing and schema hashes; model and calibration hashes; compiler/runtime versions; quantization accuracy; worst-case timing/memory; scenario and fault-injection results; coverage gaps; safety impact decision; signing/approval record; rollout and rollback plan.
