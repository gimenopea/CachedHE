# Efficient Homomorphic Encryption through Radix Caching

This repository contains the implementation, benchmarks, and analysis for evaluating the performance of **radix tree caching** on various homomorphic encryption schemes. This work supports the praxis _“Advancing Adoption of Privacy Preserving Machine Learning in AI-Enabled Enterprises”_.

## Objective

To reduce the computational cost of homomorphic encryption (HE) and enable scalable, privacy-preserving machine learning in enterprise settings through radix-based caching.


## Experimental Overview

### 1. Baseline Encryption Benchmarks (Uncached)

This praxis evaluated encryption time and homomorphic addition time across:
- **Paillier (PHE)**
- **BFV (FHE with exact arithmetic)**
- **CKKS (FHE with approximate arithmetic)**

**Key findings:**
- BFV outperformed CKKS and Paillier in raw encryption.
- Paillier had the fastest homomorphic addition.
- Doubling key size significantly impacted Paillier’s performance but had less effect on BFV.

### 2. Cached Encryption Benchmarks (Radix Tree Caching)

Radix caching decomposes plaintext into digits and reuses encrypted radix weights. Implemented using:
- A Python dictionary structure
- No size cap or eviction during benchmarks

**Speedup:**
- **Paillier:** Up to 118x improvement
- **BFV/CKKS:** Major reductions in encryption latency
- **Statistical significance:** All improvements p < 0.01 (Welch’s t-test)

### 3. Scaling Tests

This praxis tested both **strong** and **weak** scaling on:
- 4, 8, 16, and 32-core setups

**Insights:**
- Cached encryption showed near-linear strong scaling and excellent weak scaling
- Uncached encryption scaled well only under strong scaling
- Radix caching significantly reduces runtime per core

### 4. Statistical Significance (t-tests)

Welch's t-tests confirmed statistically significant performance gains in:
- **Encryption time** (all schemes and core counts)
- **Homomorphic addition time**

Tables of results are included in the `results/` directory.

### 5. Pima Diabetes Dataset (Encrypted Inference)

**Goal:** Secure logistic regression on healthcare data

- Dataset: 768 samples, 8 features
- Encryption: Paillier, BFV, CKKS with/without caching
- Parallel configurations: 4–32 cores

**Outcome:**
- Caching significantly reduced both encryption and inference time for BFV and CKKS
- Paillier showed minimal benefit due to smaller workload size

### 6. FVC2002 Fingerprint Biometrics

**Goal:** Perform privacy-preserving biometric authentication

- Scheme: Paillier with radix2 and radix10 caching
- Operations: Hamming weight over encrypted vectors

**Results:**
- Equal Error Rate (EER) unchanged (~0.49)
- Cached schemes reduced encryption and match time without loss of accuracy

---


## ⚙️ Dependencies

- Python 3.9+
- [TenSEAL](https://github.com/OpenMined/TenSEAL)
- NumPy, Pandas, Matplotlib
- OpenMPI or multiprocessing (for parallel benchmarks)

---

## Key Takeaways

- **Radix caching** reduces HE encryption time from quadratic to near-linear.
- **Lattice-based schemes** (BFV/CKKS) scale better and benefit more from caching than Paillier for large workloads.
- **Practical deployments** (healthcare, biometrics) can benefit from real-time encrypted inference using cached HE.
