---
layout: post
title: "Feature transformation: its relationship to feature maps, spaces, subspaces, and axes"
date: 2026-07-24
categories: [nlp, quantum-ml]
---
 

## Background — setting the relationship before anything else

**Feature transformation** is the general term: any operation $$T: \mathcal{X} \to \mathcal{X}'$$ that changes how data is represented. **Feature map** ($$\phi$$, the subject of the companion article) is one *specific kind* of feature transformation — the kind whose job is to realize an inner product ($$\phi(x)$$ chosen so that $$k(x,x') = \langle\phi(x),\phi(x')\rangle$$ recovers a target kernel), typically by *expanding* into a bigger, often implicit space.

$$\textbf{Feature map} \ \subset\ \textbf{Feature transformation}$$

That containment is the whole relationship, stated as plainly as possible: every feature map is a feature transformation; most feature transformations are not feature maps.

**And the direct answer to which one owns spaces/subspaces/axes: feature transformation does, decisively — and by construction, not by convention.** A feature map's relationship to "space" is existential — it proves a space with the right separability properties exists, and the kernel trick is specifically engineered so you never have to name that space's coordinates. A feature transformation's relationship to axes and subspaces is definitional — rotation, scaling, and projection *are*, literally, operations on coordinate axes and on the subspaces those axes span. The rest of this article makes that case with the formulas.

---

## 1. Classical feature transformations

### 1.1 Per-axis scaling — the shallowest possible axis operation

$$x_i' = \frac{x_i - \mu_i}{\sigma_i}$$

Every coordinate axis is rescaled independently, using only that axis's own statistics. No cross-axis information is used — a transformation that touches axes one at a time.

### 1.2 PCA — literally finding new axes, then keeping a subspace of them

$$T(x) = W^\top(x - \mu), \qquad W = [v_1, \dots, v_k],\ \ \Sigma v_i = \lambda_i v_i,\ \ \lambda_1 \ge \cdots \ge \lambda_d$$

The eigenvectors $$v_i$$ of the covariance matrix $$\Sigma$$ *are* the new axes (the principal axes). Keeping only the top $$k$$ is not a side effect — it is the explicit selection of the $$k$$-dimensional subspace $$\mathrm{span}(v_1,\dots,v_k) \subset \mathbb{R}^d$$ that carries the most variance. Subspace selection is not incidental to PCA; it is the entire mechanism.

### 1.3 Whitening — equalizing along the new axes

$$T(x) = \Sigma^{-1/2}(x - \mu)$$

Rotates onto the principal axes (as in PCA) *and* rescales along each so that the covariance becomes the identity in the new frame — every axis now carries equal, unit variance.

### 1.4 Where feature maps nest inside this

$$\phi_{\text{poly}}(x) = (1, x_1, \dots, x_n, x_1x_2, \dots), \qquad k(x,x') = \exp(-\gamma\|x-x'\|^2) \Rightarrow \phi_{\text{RBF}}(x)\ \text{implicit}$$

These are feature transformations too — by the containment stated in §0 — but notice what's absent compared to §1.1–1.3: no axis is ever named, no subspace is ever selected. The RBF map's target space doesn't have interpretable axes at all; the kernel trick is specifically designed so you never need any.

---

## 2. Quantum feature transformations

### 2.1 Closed system — unitary basis change

$$\rho \mapsto U\rho U^\dagger$$

A unitary is the quantum analogue of an orthogonal rotation — literally a rotation of the axes of Hilbert space, preserving all inner products (norms, angles) exactly the way a classical orthogonal matrix does in §1.2.

**Quantum PCA** (Lloyd–Mohseni–Rebentrost) makes the axis language exact rather than metaphorical:
$$\rho = \sum_j \lambda_j |\chi_j\rangle\langle\chi_j|$$
Repeated density-matrix exponentiation ($$e^{-i\rho\Delta t}$$, applied via a SWAP-based construction) combined with phase estimation extracts the eigenvalues $$\lambda_j$$ and eigenvectors $$|\chi_j\rangle$$ of an unknown $$\rho$$ — the quantum state's own principal axes — without ever needing full state tomography. This is the direct quantum sibling of §1.2, axis for axis.

### 2.2 Open system — transformation of the *accessible* subspace

$$\rho(x) = \sum_k K_k(x)\,\rho_0\,K_k(x)^\dagger$$

A CPTP map doesn't just rotate axes — it can reduce which subspace is even reachable at all. Engineered dissipation driving a system toward a decoherence-free subspace (from the companion article) is, read this way, a feature *transformation* whose entire effect is subspace selection: input information restricted, by physics rather than by an explicit projection matrix, onto the surviving subspace.

---

## 3. The direct comparison

| | Relationship to **space** | Relationship to **subspace** | Relationship to **axis** |
|---|---|---|---|
| **Feature map** ($$\phi$$) | Existential — proves a separating space exists | Rare, incidental (only in special cases like a DFS-restricted map) | Deliberately absent — the kernel trick exists precisely to avoid naming axes |
| **Feature transformation** ($$T$$) | Definitional — a transformation *is* a map between two coordinatized spaces | Constitutive — subspace selection (PCA truncation, dissipative restriction) is often the entire point | Constitutive — rotation, scaling, whitening are axis operations by definition |

---

## 4. Why the asymmetry holds

A feature map is allowed to succeed without you ever knowing what its axes mean — that's not a limitation, it's the design goal: Mercer's theorem guarantees $$\phi$$ exists and the kernel trick guarantees you never need to construct it. A feature transformation offers no such shortcut. Its entire deliverable — a rotation, a rescaling, a projection — is stated *in terms of* axes and subspaces of an already-coordinatized space; strip those away and there is nothing left to call a transformation. That's the structural reason feature transformation, not feature map, is the concept doing the load-bearing work whenever the question is really about spaces, subspaces, and axes — classically via PCA/whitening, quantumly via unitary rotation and QPCA's own eigenbasis, and via dissipative subspace restriction on the open-system side.

