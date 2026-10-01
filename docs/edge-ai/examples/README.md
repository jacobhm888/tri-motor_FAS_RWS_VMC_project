# Synthetic examples

## Surface recognition without video

NXP supplies a configured three-class result and confidence from chassis signals. Bind the returned labels to a versioned class map; the actual three labels must be confirmed rather than guessed. Check feature age, source validity, OOD and excitation. Use the class to select an approved prior or roughness schedule. No class-only upward mu correction is permitted.

For mu_physics=0.60, delta_mu=+0.08, cap=0.05 and weight=0.4, an eligible result yields 0.62 before projection. A low-excitation result yields 0.60 regardless of class confidence. A stale result selects physics in the current control cycle. See [safety bounds](../safety-bounds.md).

## Front-to-rear road transfer

With a synthetic 3.0 m wheelbase at 15 m/s, geometric transit time is 200 ms. A measured 30 ms sensing/processing path would leave about 170 ms before rear-wheel encounter on a matching path. Turns, acceleration, different tracks and wheel lift can invalidate the transfer. Below the validated speed threshold, disable it. These example dimensions and times are not platform measurements.

## Interrupted update

Release B downloads while approved release A remains active. Power fails during the inactive-slot write. On reboot, the activation journal still selects A. If B later passes signature, compatibility and golden-vector checks, activate the complete B tuple in a permitted parked condition and start shadow mode. A runtime fault disables B's correction and invokes the reviewed recovery policy.

## Sample manifest

[release-manifest.json](release-manifest.json) documents proposed metadata. It contains no payload or signature and declares installable=false. It must be rejected by any production installer. A production manifest format, canonical signature encoding and schema are separate implementation work.
