# Photon coil stage

Open `photon_stage.html` directly in a browser. No server, packages, network, or QPU account is required. Step one photon through the stages, run repeated preparations, collect 1,000 shots, and export the current ideal circuit as OpenQASM 3. Changing a control clears counts so distinct settings are not mixed.

Optical endpoints: PHOTON_IN / PREP → COIL_A / MATERIAL → COIL_B / MATERIAL → OPTICAL_PHASE → COIL_C / MATERIAL → ANALYZER → DETECT_OUT → RESULT_OUT. Electrical control is separate: each signed drive current acts on its modeled coil/material section. Detector results terminate a photon; repeat prepares a fresh photon. There is no detector upstream of the optical gates.

The photon carries one polarization qubit: H = |0>, V = |1>. Each material section applies Ry(2kI). A separate optical retarder applies Rz(phase). The analyzer applies Ry(-2 angle), followed by H/V measurement. Total efficiency is a polarization-independent loss sample; exported QASM represents only the ideal lossless circuit. Amplitudes shown in the UI are simulator state, not physically available nondestructive probe readings.

The default k = 1 rad/A is hypothetical, not a component specification. Physically θ = V∫B_parallel dl; a measured k must incorporate material, wavelength, geometry and field/current calibration. k = 0 removes magneto-optic coupling. Signed current requires bipolar drive, which the initial single-MOSFET schematic does not provide. This model omits coil transients, saturation, heating, source statistics, dark counts, optical loss by section, and entangling operations. It is a single-qubit numerical stage, not a constructed or validated quantum computer. Polarization probabilities alone do not establish nonclassical light.

Gate definitions were checked against this fork's `examples/stdgates.inc` at 7fbf9e9eb3692a1288c014d6efd43523701886c6. Faraday relation: https://sciencedemonstrations.fas.harvard.edu/presentations/faraday-rotation . The source supports the rotation relation, not the hypothetical default calibration.

Run `node test_photon_stage.cjs`. Tests cover polarization rotation, inverse cancellation, phase interference, norm conservation, deterministic detection/loss boundaries, controls resetting counts, input validation, and repeated stage traversal. Visual browser/device verification has not been performed. QASM export is generated using the repository's gate names; no external QASM parser or physical backend was run.
