# QSolver — Scaling the Generative Quantum Eigensolver (GQE) to 40 Qubits

**Team Name:** QSolver
**Participant:** Archana Singh
**Challenge track:** Global Industry Challenge 2026 — Mitsubishi Chemical & AIST
(`gic-2026-Mitsubishi-AIST`, https://aqora.io/challenges/global-industry-challenge-2026/tracks/gic-2026-Mitsubishi-AIST)
**Repository:** [https://github.com/ArchanaSingh1995/Potomac_Mitsubishi_Challenge.git](https://github.com/archana070723/Mitsubishi2026.git)

[![Launch on qBraid](https://qbraid-static.s3.amazonaws.com/logos/Launch_on_qBraid_white.png)](https://account.qbraid.com/api/auth/partner/aqora/start?hackathonId=gic-2026&challengeId=gic-2026-mitsubishi-aist)

## Project title

Scaling the Generative Quantum Eigensolver to 40 Qubits via MPS Simulation and Transformer Circuit Generation, targeting the singlet–triplet gap of 4CzIPN-class TADF emitters (Mitsubishi Chemical OLED materials).

## What this repository contains

- `source_code/GQE_MPS_Transformer_final.ipynb` — end-to-end pipeline: PySCF Hamiltonian construction → symmetry-preserving Givens ansatz → CUDA-Q `tensornet-mps` / NumPy energy evaluation → Transformer generator → GQE training → transfer-learning curriculum → benchmarking → gated 40-qubit run.
- `reference/Mitsubishi_Phase_2.pdf` — our Phase 2 proposal (background, method, and full resource/architecture design). Included for context; it is **not** the Phase 3 write-up.

## Setup instructions (qBraid)

1. Launch this repo on qBraid using the button above, or clone it directly into a qBraid GPU environment.
2. Run the install cell (`%pip install -q cudaq pyscf openfermion openfermionpyscf torch matplotlib "qci-client>=4"`) once per instance.
3. If you want to exercise the QCI Dirac-3 client cell, set your own credentials — do **not** reuse anyone else's token:
   ```
   %env QCI_API_URL=https://api.qci-prod.com
   %env QCI_TOKEN=<your_own_token>
   ```
4. Run cells top to bottom. Cells are labeled by module (0. Environment setup, 1. Physics smoke test, 2. Hamiltonian builder, 3. Givens ansatz, 4. Energy evaluation, 5. Transformer generator, 6. GQE training, 7. Small-system run, 8. Transfer-learning curriculum, 9. Benchmarking, 10. 40-qubit run (gated), 11. Noise-aware extension (sketch)).

## Expected inputs/outputs

- **Input:** no external data files — molecular geometries for H2, LiH, BeH2, H2O are hardcoded in the `CURRICULUM` dict; the 4CzIPN geometry is intentionally left unset (see Known Limitations).
- **Output:** printed energy-convergence logs per molecule, a two-panel matplotlib convergence/energy plot, and (when the curriculum cell completes) a benchmark table with per-system qubit count, GQE vs. FCI energy, error in mHa, chemical-accuracy flag, 2-qubit gate count, and entanglement entropy.

## Known limitations and assumptions (honest status as of this submission)

- **Physics correctness is validated**: the pure-NumPy smoke test (Module 1) reproduces the FCI ground state of H2/STO-3G to 0.0007 mHa, confirming the Givens-ansatz formulation is correct.
- **QCI Dirac-3 connectivity is validated**: the client successfully authenticates and returns a live allocation balance.
- **GQE training on H2/LiH does not currently reach chemical accuracy in this run**: energy plateaus at ~20.6 mHa error after step 20 rather than converging further, for both the standalone run (§7) and the curriculum warm-start (§8). This is short of both the 1.6 mHa chemical-accuracy bar and our Phase 2 proposal's projected <0.01–0.5 mHa targets for these systems. We believe this points to the optimizer/learning-rate schedule or the finite-difference gradient step size stalling early rather than a flaw in the ansatz itself (the smoke test shows the exact ansatz can reach the true minimum), but we have not root-caused it in time for this submission.
- **The transfer-learning curriculum cell does not complete**: it raises an `AssertionError` partway through and does not finish BeH2/H2O, which means the downstream benchmark table also fails to run in this snapshot.
- **The 40-qubit 4CzIPN run is gated and not executed**: `RUN_40Q = False`. It requires a DFT/B3LYP/6-31G*-optimized geometry (via AVAS active-space selection) that has not yet been generated, plus an A100 GPU instance. This remains the scaling target described in the Phase 2 proposal, not a result this snapshot produces.
- **No classical baseline (VQE/DMRG) is executed in this snapshot.** The code paths described in the proposal (`energy(...)` with a Ry+CNOT ansatz for VQE; ITensor for DMRG) are documented but not wired into a runnable cell here.

We are reporting these gaps directly rather than presenting numbers that don't reproduce, per the challenge's reproducibility and honesty guidance.
