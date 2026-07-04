# Image Processing in Quantum Computers — FRQI Implementation

A from-scratch Qiskit implementation of the **Flexible Representation of Quantum Images (FRQI)**
encoding scheme, following the walkthrough in *"Image Processing in Quantum Computers"*
(Dendukuri & Luu, 2018), which itself builds on the original FRQI paper by
Le, Dong & Hirota (2011).

**Full write-up:** [Implementing "Image Processing in Quantum Computers" in Qiskit](https://medium.com/@arpitha.rajeev37/image-processing-in-quantum-computers-a76ee63f5041)

**Paper implemented:** [arXiv:1812.11042](https://arxiv.org/abs/1812.11042)

## What's in this notebook

A tiny 2×2 grayscale image is encoded into a 3-qubit quantum state (2 position qubits + 1 color
qubit), measured, and decoded back into pixel values — end to end, entirely in simulation.

1. **Phase 1 — Encoding:** Convert classical pixel intensities into rotation angles and build the
   FRQI state using Hadamard gates and multi-controlled `RY` gates.
2. **Phase 2 — Measurement & Retrieval:** Simulate the circuit with `AerSimulator`, measure it, and
   statistically reconstruct pixel intensities from the measurement counts.
3. **Phase 3 — Negation:** Apply a spatial-domain image processing operation (intensity inversion)
   directly at the circuit level and verify the recovered result matches `255 - original`.

## Results

<img width="511" height="97" alt="image" src="https://github.com/user-attachments/assets/6a89f155-e247-4a39-ada0-d189c6d52c2b" />

<img width="997" height="257" alt="image" src="https://github.com/user-attachments/assets/010b1889-58e2-4cd2-a993-8383ee00f2e3" />

<img width="825" height="576" alt="image" src="https://github.com/user-attachments/assets/bce28d29-1f8a-4864-a917-3265581d084b" />

<img width="1100" height="519" alt="image" src="https://github.com/user-attachments/assets/067ac895-8da5-49de-be38-cb78dc019e2f" />

## Dependencies

```
qiskit
qiskit-aer
matplotlib
numpy
pylatexenc
jupyter
```

## Setup

```bash
pip install -r requirements.txt
jupyter notebook Image_Processing_in_Quantum_Computers.ipynb
```

Then run all cells top to bottom.

## A note on bit ordering

Qiskit measurement bitstrings are read most-significant-bit first (`c2 c1 c0`). This notebook's
encoding circuit addresses coordinates using a `(q0)(q1)` convention, so the decoding step reassembles the position key accordingly. Getting
this backwards is an easy, silent bug — it swaps recovered pixel values between non-symmetric
coordinates (`01` ↔ `10`) while the symmetric ones (`00`, `11`) still look correct, which can hide
the mistake unless you check every coordinate individually.

## What's next: stepping into the Fourier basis

This notebook only covers spatial-domain operations. A natural extension is appending a Quantum
Fourier Transform (QFT) block onto the position qubits, shifting the encoded image from the
computational basis into the Fourier basis — the step that underlies the paper's central claim
of exponential speedup over classical FFT-based image processing. Contributions welcome!

## References

- P.Q. Le, F. Dong, K. Hirota. *"A flexible representation of quantum images for polynomial
  preparation, image compression, and processing operations."* Quantum Information Processing,
  10(1), 63–84 (2011).
- A. Dendukuri, K. Luu. *"Image Processing in Quantum Computers."* [arXiv:1812.11042](https://arxiv.org/abs/1812.11042) (2018).

## License

No license specified — shared as sample/learning code implementing the referenced papers.
