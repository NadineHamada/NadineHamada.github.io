---
layout: post
title: "The Hidden Boundary in Machine Learning:The Four Stages of a Hypothesis Space"
date: 2026-08-03
categories: [Statistical Learning Theory, VC dimension, inductive bias, No-Free-Lunch, and generalization]
---


Everything in this piece hangs off one table. Each row below gets its own section — the table is the skeleton, not a summary.

| Stage | Needs data? |
|---|---|
| $H$ gets **constructed** (feature transformation) | No — architectural choice |
| $H$ gets **described** (VC dimension, raw capacity) | No — a property of $H$ alone |
| $H$ carries a checkable **inductive bias** | **Yes** — meaningless without a train/unseen split |
| $H$ gets **operated within** (gradient descent) | Yes — needs actual training data |

The line splitting "no" from "yes" falls in the middle, between rows two and three — not, as intuition might suggest, before row one. $H$ can be fully built and fully measured before a single data point exists. Only the *third* stage, where "bias" as a concept becomes meaningful at all, requires data to enter.

---

## Stage 1 — H gets constructed

This is feature transformation's job, covered in its own article: $\phi$ or $T$ builds the concrete space $H$ operates over. $H = \{\text{all CNNs with this weight-sharing pattern}\}$, $H = \{\text{all depth-}d\text{ trees, axis-aligned splits}\}$, $H = \{f(x) = w^\top x + b\}$ — every one of these is a whiteboard exercise. No photos, no labels, no dataset in the room.

## Stage 2 — H gets described

VC dimension, Rademacher complexity, covering numbers — the capacity-measurement machinery from the very start of this whole conversation. These take an *already-built* $H$ and answer "how big is this," with zero reference to any specific dataset. A linear classifier's VC dimension in $\mathbb{R}^d$ is $d+1$ whether or not you've ever trained one.

## Stage 3 — H carries a checkable inductive bias

This is where "no" flips to "yes," and it's worth being precise about *why*: "bias" specifically means a prediction about behavior on unseen data. A prediction needs something to be predicted about — so the claim is empty, not wrong, until a train/unseen split exists.

**The pattern across architectures — the same story, every time:**

- **CNN:** $H$ = weight-sharing pattern, buildable data-free. "Biased toward translation-invariant patterns" is empty until unseen images exist to test it against.
- **Decision trees:** $H = \{\text{depth-}d\text{ trees, axis-aligned splits}\}$. "Biased toward axis-aligned boundaries" only becomes checkable with data that has, say, a genuinely diagonal boundary.
- **Linear models:** $H = \{w^\top x + b\}$. "Assumes linearity" is vacuous until data could confirm or violate it.
- **k-NN:** $H$ defined by a distance metric and smoothness assumption. "Nearby points share labels" is vacuous until actual labeled points exist.
- **Variational quantum circuit ansatz:** $U(\theta)$ fully specifiable — gate layout, entangling structure — with no data anywhere near it. Whether that entangling pattern matches your actual quantum feature map's data is unknowable until real inputs run through it.

**The general rule these all collapse into:** you can always write $H$'s *shape* from pure design choices — but "shape" and "bias" aren't synonyms. Calling an architecture's structure "an inductive bias" before any train/test split exists is really just "a shape" wearing the more impressive name early. Not wrong — just unearned yet.

**The deeper wrinkle — clustering breaks even this once data arrives.** $H$ for k-means: all partitions into $k$ Voronoi regions under Euclidean distance — data-free, same as everything above. The bias claim ("prefers convex, similarly-sized clusters") is real and checkable, same structure as the rest. But here's the difference: classification has an unambiguous anchor once data exists — a true label $y$, matched or not. Clustering has no such anchor; there's often no external "correct" partition to check against at all, only whatever objective the algorithm optimizes.

This isn't just practical murkiness — it's a proven impossibility. Kleinberg's 2002 theorem shows no clustering function can simultaneously satisfy scale-invariance (rescaling all distances by a constant shouldn't change the output), richness (every partition should be achievable for some distance function), and consistency (shrinking within-cluster distances and stretching between-cluster distances shouldn't change the output) — for any $n \ge 2$. So clustering doesn't just delay when its bias becomes checkable the way the other four examples do; it calls into question whether "checkable against ground truth" is even the right frame once data does show up.

## Stage 4 — H gets operated within

Gradient descent, backprop, the parameter-shift rule on a quantum ansatz — all of it needs actual training data to move through, not just the concept of a train/unseen split. This is the same territory as the classical/quantum operating-within-$H$ table from earlier in this series: ordering ↔ amplitude amplification, combination ↔ interference, composition ↔ entangled composition, local movement ↔ tunneling. Stage 3 asks whether $H$'s shape *should* work on unseen data; stage 4 is the actual, physical process of moving through $H$ to find out.

---

## Why the boundary sits exactly where it does

The no-free-lunch theorem is the formal reason "no" flips to "yes" specifically between stages 2 and 3, not anywhere else. NFL is a statement about performance *averaged over data-generating distributions* — it cannot be stated, let alone proven, without data already being part of the setup. That's what pins the boundary in place: $H$ can be built and measured in a data-free world, but the moment you want to say anything about how it behaves on what it hasn't seen, you've left that world, whether or not you've started training yet.
