# Group-Theoretic Classification of Explainability

## Overview

The universal explanation impossibility theorem proves that no explanation of an underspecified system can simultaneously be faithful, stable, and decisive. The proof is group-free — it requires only the Rashomon property (two configurations with the same observable and incompatible explanations). But the *resolution* of the impossibility is deeply group-theoretic: the optimal strategy depends on the symmetry group G acting on equivalent configurations and its representation on the explanation space.

This document explains how explainability is parameterized by groups and representations, and what the classification predicts.

---

## The Two Layers

### Layer 1: The Impossibility (Group-Free)

The impossibility theorem requires only:
- A configuration space Θ, explanation space H, observable space Y
- Maps obs: Θ → Y and exp: Θ → H
- An incompatibility relation ⊥ on H
- The Rashomon property: ∃ θ₁, θ₂ with obs(θ₁) = obs(θ₂) and exp(θ₁) ⊥ exp(θ₂)

The 4-step proof uses no group theory:
1. Decisiveness: exp(θ₁) ⊥ exp(θ₂) → E(θ₁) ⊥ E(θ₂)
2. Stability: obs(θ₁) = obs(θ₂) → E(θ₁) = E(θ₂)
3. Substitution: E(θ₂) ⊥ E(θ₂)
4. Faithfulness: ¬(exp(θ₂) ⊥ E(θ₂)) — contradiction.

This is why the impossibility is universal: the same logical argument applies to codons, gauge fields, parse trees, DAGs, and any other domain satisfying the Rashomon property. The group structure is invisible at this layer.

### Layer 2: The Resolution (Group-Dependent)

Given that the three properties cannot coexist, how should one sacrifice decisiveness? The answer depends on the symmetry group G and its representation.

**Definition.** A group G acts on Θ by permuting configurations that produce the same observable: obs(g · θ) = obs(θ) for all g ∈ G. The *G-invariant resolution* is a map E: Θ → H that is constant on G-orbits: E(g · θ) = E(θ) for all g.

**Theorem (Pareto-optimality).** Among all stable explanation maps, the G-invariant projection is Pareto-optimal: no alternative can achieve strictly higher pointwise faithfulness on every equivalence class.

The specific form of this projection — how it is computed — depends entirely on G and the structure of H.

---

## The Classification

### Axis 1: Group Type

| Group type | Example | Resolution difficulty |
|-----------|---------|----------------------|
| Finite abelian | Z₂^{|V|-1} (gauge theory) | Unique canonical resolution |
| Finite, vector-valued H | S_k on codons (biology) | Orbit averaging (well-defined, unique) |
| Finite, combinatorial H | S_k on parse trees (linguistics) | Equivalence-class reporting (no averaging possible) |
| Compact continuous | U(1) phase (simplified crystallography) | Haar integral provides canonical average |
| Non-compact continuous | ℝ^{n-r} null-space translations (math) | Orbit average diverges; regularization required |

### Axis 2: Representation (What G Acts On)

Even when the abstract group is the same (e.g., S_k), the representation — what objects the group permutes — determines the resolution form:

| Domain | Group | Objects permuted | H structure | Resolution form |
|--------|-------|-----------------|-------------|----------------|
| Biology | S_k (k ≤ 6) | Synonymous codons | Frequency vector (ℝ^k) | Orbit averaging |
| Stat mech | S_Ω | Microstates | Thermodynamic quantities (ℝ) | Ensemble averaging |
| Linguistics | S_k | Parse trees | Discrete set | Equivalence-class enumeration |
| Computer science | S_k | Hidden DB columns | Row set | Subquery projection |

The abstract group S_k is the same in all four cases. But:
- In biology, H is a vector space (codon frequency distributions), so averaging is well-defined → codon usage tables.
- In statistical mechanics, H is a vector space (energy, magnetization), so averaging is well-defined → microcanonical ensemble.
- In linguistics, H is a set of trees with no vector space structure, so averaging is undefined → packed parse forests (enumerate the entire equivalence class).
- In computer science, H is a set of database rows, so the resolution projects onto the invariant subspace of queries → complement views.

### Axis 3: Resolution Form

The G-invariant projection takes different computational forms depending on the group and representation:

**Orbit averaging** (when H is a vector space and G is finite or compact):
```
Ē(θ) = (1/|G|) Σ_{g ∈ G} exp(g · θ)
```
This is the Reynolds operator applied to the explanation map. It projects onto the G-fixed subspace of H. Examples: codon usage tables, microcanonical ensemble averages.

**Equivalence-class reporting** (when H is a discrete/combinatorial space):
```
Ē(θ) = {exp(g · θ) : g ∈ G}   (the full orbit)
```
When H has no additive structure, the "average" is replaced by the set of all explanations consistent with the observation. Examples: packed parse forests (the set of all valid parse trees), CPDAGs (the partially directed graph encoding all Markov-equivalent DAGs).

**Invariant function extraction** (when a derived G-invariant quantity exists):
```
Ē(θ) = f(exp(θ))   where f is G-invariant
```
Rather than averaging or enumerating, one computes a function of the explanation that is already constant on orbits. Examples: Wilson loops/holonomy in gauge theory (the product around a closed loop is gauge-invariant), Patterson maps in crystallography (the autocorrelation |F(k)|² is phase-invariant).

**Regularized projection** (when G is non-compact):
```
Ē(θ) = argmin_{x ∈ orbit(θ)} ‖x‖   (or Tikhonov: argmin ‖x‖² + λ‖Ax - b‖²)
```
When G is non-compact (e.g., ℝ^{n-r}), the orbit average integral diverges. The resolution selects a canonical representative of each orbit using a regularization criterion. Example: the pseudoinverse selects the minimum-norm solution, which is the closest point to the origin in the solution set.

---

## Predictive Power

The group-representation classification generates testable predictions:

### Prediction 1: Resolution form follows from H's structure

If you discover a new domain where the explanation space is a vector space and the symmetry group is finite, the framework predicts the optimal resolution will be an averaging operation — even if no one in that domain has tried it yet. If the explanation space is combinatorial, the framework predicts the resolution will be an equivalence-class representation.

**Test:** Find a domain where practitioners haven't yet settled on a resolution strategy. Classify its group and representation. Predict the resolution form. Check if practitioners eventually converge on it.

### Prediction 2: Non-abelian groups predict resolution multiplicity

When G is non-abelian (S_k for k ≥ 3), the orbit average may depend on the order of averaging, and multiple reasonable G-invariant maps may exist. For abelian groups (Z₂, ℝ^d), the resolution is canonical.

**Observation:** There is more methodological disagreement about codon usage metrics (biology, S_6 non-abelian) — CUB, RSCU, ENC, tAI — than about gauge-invariant observables (physics, Z₂^{|V|-1} abelian) where the Wilson loop is essentially the only choice. The framework explains this: non-abelian symmetry creates room for multiple valid resolution strategies.

### Prediction 3: Non-compact groups predict regularization dependence

The mathematics instance (ℝ^{n-r}) is the only one with a non-compact symmetry group, and it is the only one where the resolution requires an arbitrary choice (minimum-norm vs. Tikhonov vs. ridge, each with a regularization parameter). The framework predicts that any new domain with a non-compact symmetry group will face the same regularization ambiguity.

### Prediction 4: The hierarchy of resolution difficulty

| Group property | Consequence | Example |
|---------------|-------------|---------|
| Finite, abelian | Unique canonical resolution; trivially computable | Gauge theory (Z₂) |
| Finite, non-abelian, vector H | Unique orbit average; O(|G|) computation | Biology (S_k), Stat mech (S_Ω) |
| Finite, non-abelian, combinatorial H | Resolution multiplicity; enumeration complexity | Linguistics (S_k), Statistics (MEC) |
| Compact continuous | Haar integral exists; numerical integration | Crystallography |
| Non-compact continuous | Orbit average diverges; regularization needed | Mathematics (ℝ^{n-r}) |

This hierarchy is monotonically ordered: each successive row is strictly harder than the previous one. The framework predicts that practitioners in "easier" rows should have fewer methodological disagreements and more stable results.

---

## The Eight Instances

### Mathematics: ℝ^{n-r} (Continuous, Abelian, Non-compact)

- **Group:** Null-space translations ℝ^{n-r}
- **Objects permuted:** Offsets from particular solution along null space
- **H structure:** ℝ^n (solution vectors)
- **Resolution:** Pseudoinverse (minimum-norm projection)
- **Resolution form:** Regularized projection (non-compact group, divergent orbit average)
- **Difficulty:** Regularization-dependent (minimum-norm, Tikhonov, ridge all give different answers)

### Biology: S_k, k ≤ 6 (Finite, Non-abelian for k ≥ 3)

- **Group:** Synonymous substitution group S_k for each amino acid
- **Objects permuted:** Synonymous codons encoding the same amino acid
- **H structure:** ℝ^k (codon frequency vectors)
- **Resolution:** Codon usage tables (frequency distribution over synonymous codons)
- **Resolution form:** Orbit averaging (H is a vector space)
- **Difficulty:** Low for single amino acids; resolution multiplicity arises in codon usage indices (CUB, RSCU, ENC)

### Physics (Gauge Theory): Z₂^{|V|-1} (Finite, Abelian)

- **Group:** Gauge transformation group Z₂^{|V|-1}
- **Objects permuted:** Edge labels (gauge field values)
- **H structure:** Boolean edge configurations
- **Resolution:** Wilson loops / holonomy (gauge-invariant observables)
- **Resolution form:** Invariant function extraction (holonomy is already G-invariant)
- **Difficulty:** Minimal — abelian group, unique invariant, trivially computable

### Statistical Mechanics: S_Ω (Finite, Non-abelian for Ω ≥ 3)

- **Group:** Permutation group of microstates with fixed macrostate
- **Objects permuted:** Microstates (molecular configurations)
- **H structure:** ℝ (thermodynamic observables: energy, magnetization, entropy)
- **Resolution:** Microcanonical ensemble (uniform average over microstates at fixed macrostate)
- **Resolution form:** Orbit averaging (H is real-valued)
- **Difficulty:** Low for thermodynamic quantities; computational cost of enumeration grows combinatorially with system size

### Linguistics: S_k (Finite, Non-abelian for k ≥ 3)

- **Group:** Permutation of valid parse trees for a given surface string
- **Objects permuted:** Parse trees (constituency bracketings)
- **H structure:** Discrete set of trees (no vector space structure)
- **Resolution:** Packed parse forests (compact representation of all valid parse trees)
- **Resolution form:** Equivalence-class enumeration (cannot average trees)
- **Difficulty:** Moderate — enumeration may be exponential in sentence length; packed forests provide polynomial-space representation

### Crystallography: Z₂ × Z_n × Z₂ (Finite)

- **Group:** Trivial ambiguity group for real-signal phase retrieval (sign, circular shift, reversal)
- **Objects permuted:** Signal phases / electron density maps
- **H structure:** Complex-valued signals
- **Resolution:** Patterson map (autocorrelation function, phase-invariant)
- **Resolution form:** Invariant function extraction (autocorrelation is G-invariant)
- **Difficulty:** Low for extracting invariants; reconstructing full structure from Patterson map requires additional constraints (positivity, atomicity)

### Computer Science: S_k (Finite, Non-abelian for k ≥ 3)

- **Group:** Permutation of hidden column values consistent with the visible projection
- **Objects permuted:** Hidden database columns
- **H structure:** Set of database rows (no vector space structure)
- **Resolution:** Complement views (restrict queries to the unambiguous projection)
- **Resolution form:** Subquery projection (restrict to G-invariant queries)
- **Difficulty:** Low for query restriction; complete recovery requires complement views satisfying lossless join decomposition

### Statistics (Causal Inference): MEC Permutations (Finite, Varies)

- **Group:** Permutations of DAGs within the Markov equivalence class (edge reversals preserving CI structure)
- **Objects permuted:** DAG edge orientations
- **H structure:** Discrete set of DAGs (no vector space structure)
- **Resolution:** CPDAG (completed partially directed acyclic graph — the shared skeleton with only forced orientations)
- **Resolution form:** Equivalence-class reporting (the CPDAG IS the equivalence class)
- **Difficulty:** Moderate — computing the MEC from observational data requires constraint-based or score-based algorithms; the number of DAGs in a MEC can be exponential in the number of nodes

---

## Connection to Representation Theory

The classification above is an instance of the general problem in representation theory: given a group G acting on a space V, decompose V into irreducible representations and identify the G-fixed subspace V^G (the trivial representation).

- **The G-fixed subspace V^G** consists of all elements v ∈ V such that g · v = v for all g ∈ G. This is exactly the space of G-invariant explanations — the stable explanations.
- **The Reynolds operator** R: V → V^G given by R(v) = (1/|G|) Σ_{g ∈ G} g · v projects onto V^G. This is the orbit average.
- **The orthogonal complement (V^G)⊥** consists of the "decisive" information that is lost by the projection. This is the information the resolution sacrifices.

For finite groups acting on vector spaces, Peter-Weyl theory gives a complete decomposition:
```
V = V^G ⊕ V₁ ⊕ V₂ ⊕ ... ⊕ V_r
```
where each V_i is an irreducible representation of G. The resolution projects onto V^G and discards V₁, ..., V_r. The amount of information lost is determined by the dimensions of the non-trivial representations.

For the explanation impossibility: **faithfulness** asks E(θ) to live in or near the full space V. **Stability** requires E(θ) ∈ V^G. **Decisiveness** requires E to preserve distinctions, which means E must have non-trivial components in V₁, ..., V_r. The impossibility is that V^G and (V^G)⊥ are orthogonal complements — you cannot project onto V^G while retaining components in (V^G)⊥.

This perspective reveals that the **amount of information lost** in the resolution depends on the representation-theoretic structure:
- If V^G is large (most of V), little information is lost — the system is "mostly identifiable."
- If V^G is small (e.g., just the constant functions), most information is lost — the system is "mostly underspecified."
- The dimension of V^G relative to V is a quantitative measure of the "severity" of the impossibility for each domain.

---

## Summary

The explanation impossibility is logically simple (4 steps, group-free). The resolution theory is where the mathematics lives. The group G and its representation on the explanation space H determine:

1. **Whether a resolution exists** — always yes (the G-invariant projection)
2. **Whether it is unique** — yes for abelian groups; possibly no for non-abelian groups
3. **What computational form it takes** — averaging, enumeration, invariant extraction, or regularized projection
4. **How much information is lost** — determined by dim(V^G) / dim(V)
5. **What difficulties practitioners face** — finite groups are easy; non-compact groups require regularization choices

The fact that eight independent scientific fields, over more than a century, converged on the resolution form predicted by their respective group-representation structures — without awareness of one another's work — is the empirical evidence that this classification is doing real mathematical work, not just relabeling known phenomena.
