---
layout: post
title: "Feature maps across classical and quantum learning: a formal classification"
date: 2026-07-31
categories: [Computational Learning Theory]
---

## The pivot equation

Every section below returns to one inequality:

$$\rho(x) \neq |\phi(x)\rangle\langle\phi(x)|$$

The right-hand side is a **pure state** — a feature map that lives on a single coherent trajectory through Hilbert space. The left-hand side is a **mixed state** — a feature map that has, at some point, been averaged over an inaccessible environment. Classical feature maps never face this fork at all: a classical representation is just a vector, full stop, with no pure/mixed distinction to make. That absence is itself the first data point in the classification that follows.

---

## 1. Classical feature maps

### 1.1 Deterministic

A fixed, hand-specified function $$\phi: \mathbb{R}^n \to \mathbb{R}^d$$ (or into an implicit space), chosen by design, not by data.

**Polynomial features** (finite, explicit):
$$\phi(x) = (1,\ x_1, \dots, x_n,\ x_1x_2, \dots, x_i x_j, \dots)$$

**RBF / Gaussian kernel feature map** (infinite, implicit — never instantiated):
$$k(x,x') = \exp(-\gamma\|x-x'\|^2) = \langle \phi(x), \phi(x')\rangle_{\mathcal{H}}, \qquad \dim \mathcal{H} = \infty$$
Mercer's theorem guarantees $$\phi$ exists; the kernel trick means you compute $k$ directly and never write $\phi(x)$ down.

**Random Fourier features** (finite, explicit — a sampled approximation to the infinite RBF space):
$$\phi(x) = \sqrt{\tfrac{2}{D}}\big[\cos(\omega_1^\top x + b_1), \dots, \cos(\omega_D^\top x + b_D)\big], \quad \omega_i \sim \mathcal{N}(0, 2\gamma I)$$

### 1.2 Learned

$\phi_\theta$ is a parametrized function, $\theta$ fit by gradient descent against a loss — the encoder half of an autoencoder, a CNN backbone, a transformer's embedding stack.

**Autoencoder encoder**:
$$\phi_\theta(x) = \sigma(W_L\, \sigma(\cdots \sigma(W_1 x + b_1)\cdots) + b_L), \qquad \theta^\star = \arg\min_\theta \|x - \mathrm{dec}_\theta(\phi_\theta(x))\|^2$$

**Contrastive / self-supervised embedding** (no reconstruction target at all):
$$\theta^\star = \arg\min_\theta\ \mathcal{L}_{\text{InfoNCE}}\big(\phi_\theta(x), \phi_\theta(x^+), \{\phi_\theta(x^-_i)\}\big)$$

**Learned lookup embedding** (finite, explicit, learned):
$$\phi(w) = E[\,\mathrm{idx}(w)\,], \qquad E \in \mathbb{R}^{|V|\times d} \text{ trained end-to-end}$$

### 1.3 Why classical feature maps are organized this way

The two axes that matter classically — *deterministic vs. learned* (where did $\phi$ come from) and *finite/infinite, explicit/implicit* (how large is the target space, and do you ever materialize it) — are the only two axes that need to exist, because a classical vector, once computed, is a stable, faithfully-copyable object. There is nothing about *evaluating* $\phi(x)$ that can degrade $\phi(x)$. So the only open questions are about the map's origin and its size. No physical process threatens the representation once it exists.

---

## 2. Quantum feature maps — closed systems

**General form**, unitary and information-preserving:
$$x \mapsto |\phi(x)\rangle = U(x)|0\rangle^{\otimes n}$$

This is a pure state: $\rho(x) = |\phi(x)\rangle\langle\phi(x)|$, i.e. the pivot inequality is an *equality* here — the special case where nothing has been lost.

**Angle encoding**:
$$|\phi(x)\rangle = \bigotimes_{i=1}^n R_y(x_i)|0\rangle = \bigotimes_{i=1}^n \big(\cos\tfrac{x_i}{2}|0\rangle + \sin\tfrac{x_i}{2}|1\rangle\big)$$

**Amplitude encoding**:
$$|\phi(x)\rangle = \frac{1}{\|x\|}\sum_{i=0}^{2^n-1} x_i |i\rangle$$

**IQP / ZZ feature map** (Havlicek et al.):
$$U_\Phi(x) = \exp\Big(i\!\!\sum_{S \subseteq [n],\, |S|\le 2}\!\! \phi_S(x) \prod_{k\in S} Z_k\Big), \qquad |\phi(x)\rangle = U_\Phi(x)H^{\otimes n}U_\Phi(x)H^{\otimes n}|0\rangle^{\otimes n}$$

**Data re-uploading** — the closed-system bridge back to "learned":
$$|\phi(x,\theta)\rangle = U(\theta_L)U(x)\,U(\theta_{L-1})U(x)\cdots U(\theta_1)U(x)|0\rangle$$
Data and trainable parameters are interleaved; $\theta$ is optimized exactly like a classical learned map, while the map stays unitary throughout. This shows deterministic/learned is *not* erased by the quantum setting — it nests inside the closed branch as its own sub-axis.

Kernel: $\ k(x,x') = |\langle\phi(x)|\phi(x')\rangle|^2$, evaluated via a swap test.

---

## 3. Quantum feature maps — open systems

**General form**, a CPTP map — here the pivot inequality is a genuine, structural inequality:
$$x \mapsto \rho(x) = \sum_k K_k(x)\, \rho_0\, K_k(x)^\dagger, \qquad \sum_k K_k(x)^\dagger K_k(x) = I$$

**Lindblad steady-state encoding** — the feature *is* a fixed point of dissipative dynamics:
$$\frac{d\rho}{dt} = -i[H(x),\rho] + \sum_j \Big(L_j \rho L_j^\dagger - \tfrac12\{L_j^\dagger L_j, \rho\}\Big), \qquad \rho(x) \equiv \rho_{ss}(x) = \lim_{t\to\infty}\rho(t)$$
Used directly in dissipative quantum classifiers, where the classification result is read from the qubit's relaxed steady state rather than a projective measurement on a coherently prepared state.

**Quantum reservoir computing readout** — dissipation is the feature-generating mechanism itself, not an error to remove:
$$\text{feature vector} = \big(\langle O_1(t)\rangle,\dots,\langle O_m(t)\rangle\big) = \big(\mathrm{Tr}[O_1\rho(x,t)],\dots,\mathrm{Tr}[O_m\rho(x,t)]\big)$$
where $\rho(x,t)$ evolves under a fixed driven-dissipative Lindbladian, driven by a time-dependent classical signal $x(t)$. A perfectly unitary reservoir has no fading memory and cannot do this task at all — the openness is load-bearing.

Kernel: $\ k(x,x') = \mathrm{Tr}[\rho(x)\rho(x')]$, the Hilbert–Schmidt inner product — the natural generalization once state vectors become operators.

---

## 4. The count

| Family | Formulas given |
|---|---|
| Classical, deterministic | 3 |
| Classical, learned | 3 |
| Quantum, closed | 4 |
| Quantum, open | 3 |
| **Total canonical feature-map formulas** | **13** |

---

## 5. Why the classification axes themselves shift — the responsible component

Classically, the organizing questions are *origin* (deterministic vs. learned) and *size/representation cost* (finite/infinite, explicit/implicit). Quantumly, a third, more fundamental axis appears — closed vs. open, coherent vs. decohered — that has **no classical counterpart whatsoever**. The reason isn't a difference in taste or convention. It's a specific, nameable piece of formal machinery:

**The system–environment interaction term, $H_{\text{int}} = \sum_k A_k \otimes B_k$, and the CPTP/Kraus/Lindblad formalism built to describe what happens when you trace over it.**

This term is the component responsible for the shift. It exists in quantum mechanics because quantum states carry a physical resource — coherence, the relative phase between amplitudes — that can be irreversibly transferred into an inaccessible environment through exactly this coupling. Once that transfer happens, the state you're left holding is no longer describable as a single ket; you need the density-matrix formalism, and specifically the CPTP-map machinery, to describe it correctly. This is not a modeling inconvenience — it is a strictly larger space of physically possible processes than unitary evolution alone.

Classical probability theory has no equivalent term because a classical state — a definite outcome, or a probability distribution over definite outcomes — has no relative phase to begin with. There is nothing in $x$ or $\phi(x)$ that a classical environment could entangle with in a way that changes what $\phi(x)$ *is*. Reading a classical bit, copying a classical bit, or letting a classical bit sit in a noisy environment for a while does not, even in principle, transform it into a fundamentally different kind of mathematical object. A quantum state can be transformed into a fundamentally different kind of object — a ket becomes a density matrix — by nothing more than proximity to an environment. That asymmetry is entirely due to $H_{\text{int}}$ having quantum-mechanical content (it can generate entanglement) that a classical interaction term structurally cannot have.

So the honest summary: deterministic-vs-learned and finite-vs-infinite don't disappear in the quantum setting — data re-uploading is a learned closed map, a trained Lindbladian is a learned open map, both axes reappear intact inside each branch. What's added is a physically prior question that classical feature maps never had to answer, because classical information was never at risk of the thing that question is asking about.

| | Classical | Quantum |
|---|---|---|
| Origin axis | deterministic vs. learned | deterministic vs. learned (nested inside each branch below) |
| Size axis | finite/infinite, explicit/implicit | same axis still applies, secondary |
| **New axis** | — (no analogue exists) | **closed vs. open** — governed by $H_{\text{int}}$ and the CPTP formalism |
| What's at stake in the new axis | nothing extra | coherence: whether $\rho(x)$ stays a pure $\vert\phi(x)\rangle\langle\phi(x)\vert$ or becomes genuinely mixed |
