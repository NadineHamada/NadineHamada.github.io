---
layout: post
title: "The Recurring Eigenstructure Question"
date: 2026-07-23
categories: [nlp, quantum-ml]
---

Opening paragraph — the hook, plain text.

## A Section Heading

Body text goes here, citing a source right where the claim needs
it[^mercer1909]. Regular Markdown still works as normal: **bold**,
*italic*, [links](https://example.com), bullet lists.

Statistical learning theory's generalization bounds[^vc1971] are what
made kernel methods trustworthy, not just clever.

Inline math: $H = \sum_{i<j} J_{ij} s_i s_j$

Block math:

$$
P(x) = \left| \langle x | \psi \rangle \right|^2
$$

A code block:

```python
import torch
H = torch.linalg.eigh(hamiltonian)
```

An image, with a caption underneath:

![Concept diagram](/assets/images/your-image.png)
*Figure 1: caption text here.*

## Another Section

More content, another citation here[^aronszajn1950].

---

*Related: [Al-Mokh on GitHub](https://github.com/...) · [arXiv preprint](https://arxiv.org/...)*

## References

[^mercer1909]: J. Mercer (1909). "Functions of positive and negative type, and their connection with the theory of integral equations." *Philosophical Transactions of the Royal Society A*, 209, 415–446.
[^vc1971]: V. Vapnik & A. Chervonenkis (1971). "On the uniform convergence of relative frequencies of events to their probabilities." *Theory of Probability and Its Applications*, 16(2), 264–280.
[^aronszajn1950]: N. Aronszajn (1950). "Theory of reproducing kernels." *Transactions of the American Mathematical Society*, 68(3), 337–404.