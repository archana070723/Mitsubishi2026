# Phase 3 write-up — gap checklist (NOT a submission-ready document)

This is a working checklist, not a draft of the write-up itself. Aqora's rules flag that use of
AI-generated content in the submission may be grounds for disqualification, so the actual
5-page write-up (in the required Cover Page template, 11pt Times New Roman, single-spaced,
max 5 pages excluding references) should be written and reviewed in your own words. Use this
only to see what's missing relative to your Phase 2 proposal and the Phase 3 rubric.

## What Phase 3 explicitly requires that Phase 2 did not

- [ ] **A classical baseline comparison on the same problem instance.** The rubric calls this
      out as "the single most common gap in Phase 2 submissions." Your Phase 2 proposal names
      FCI, VQE (Ry+CNOT), and DMRG (ITensor) as planned baselines, but none are wired into a
      runnable cell yet. At minimum, FCI is already computed in the notebook (`fci_energy`) —
      that comparison can be reported now; VQE/DMRG still need implementation or an honest note
      that they weren't completed.
- [ ] **Specific numbers, not qualitative claims.** Per system: qubit count, circuit depth /
      2-qubit gate count, shot budget (if applicable — this is a statevector/tensor-network
      simulation, so say so explicitly), wall-clock runtime, and the actual energy error in mHa.
      Real numbers currently available from this run:
        - Smoke test (Module 1, exact/no training): 0.0007 mHa error vs. FCI on H2/STO-3G.
        - GQE-trained H2 (4 qubits, 2 params): converged to -1.116684 Ha, 20.586 mHa error
          (not chemical accuracy).
        - QCI Dirac-3 client: authenticated successfully, allocation balance 259,200 s.
      Numbers you do NOT yet have and should not claim: LiH/BeH2/H2O final errors (curriculum
      crashed), 40-qubit 4CzIPN energy (gated, not run), 2-qubit gate counts per system (the
      helper function exists — call `two_qubit_gate_count(n_qubits, n_layers)` and report it).
- [ ] **Explanation of what's quantum advantage vs. what's classical.** Right now the ansatz is
      simulated (statevector or MPS) rather than run on real QPU hardware — that's fine and
      expected at this scale, but the write-up should say explicitly that Phase 3 results are
      simulator-based, not hardware execution, and explain why (chemical-accuracy validation
      before spending QPU/Dirac-3 budget on a 40-qubit target).
- [ ] **Discussion of the training plateau.** This is exactly the kind of honest limitation the
      rubric rewards. Whatever the actual root cause turns out to be (learning rate, gradient
      step size, optimizer stalling, local minimum), it should be named specifically rather than
      glossed over.
- [ ] **Reproducibility statement matching what judges will actually see.** Phase 2 promised
      `python run_gqe.py --molecule 4CzIPN --qubits 40` as a reproducibility entry point — that
      script doesn't exist in the current repo. Either build it, or update the write-up to
      describe the actual entry point (the notebook itself) so the claim matches reality.

## What can likely carry over from Phase 2 largely unchanged

Section 1 (Focus Area/Rationale), Section 2.1–2.4 (GQE formulation, MPS rationale, Transformer
architecture, curriculum idea), Section 3 (architecture diagram), Section 6 (platform
justification) describe the *design*, which hasn't changed. These can be condensed/carried
forward — Phase 3's added value should mostly go into Sections 4–5 (actual data/results) and a
new limitations section, within the 5-page cap.

## Logistics checklist

- [ ] Use the official Phase 3 Cover Page template (`GIC_2026 Cover Page.docx`, linked on the
      Aqora challenge page) — required for compliance.
- [ ] 11pt Times New Roman, single-spaced, max 5 pages excluding references.
- [ ] Final zip filename: `TeamName_Challenge_Phase3.zip` (this package is currently named
      `QSolver_Mitsubishi-AIST_Phase3.zip` — confirm the exact "Challenge" token Aqora expects,
      e.g. it may want `QSolver_Mitsubishi_Phase3.zip` — check the submission form/instructions
      for the exact required string).
