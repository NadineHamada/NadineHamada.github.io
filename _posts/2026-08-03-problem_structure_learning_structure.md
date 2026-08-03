---
layout: post
title: "Problem Structure and Learning Structure"
date: 2026-08-03
categories: [Statistical Learning Theory, VC dimension, inductive bias, No-Free-Lunch, and generalization]
---

### Before You Ask About H: Finding the Right Object

It's the problem itself, not $$H$$. $$H$$only shows up in one of the four rows below, and even there it's not $H$ alone that carries the structure — it's the relationship between $$H$$ and the data.

## What actually carries the structure, row by row

| Row | What possesses the structure | Does $$H$$ even exist here? |
|---|---|---|
| **Type A** | The pair $$(f, G)$$ — the oracle function and the algebraic group it's defined over. Structure = $$f$$'s invariance under $$G$$'s cosets | **No.** Factoring has no hypothesis space, no training data, no generalization — it's one fixed computational task |
| **Type B** | The physical process / unitary $U$ generating the sampling distribution. Structure = that distribution's probabilities equal a permanent/hafnian | **No.** Boson sampling isn't learning anything from data either |
| **Inductive bias** | The *relationship* between $$H$$ (your model class) and the data-generating distribution's symmetries | **Yes** — this is the only row where $$H$$ is even the right object to talk about |
| **Outside the table** | The physical system being simulated, or the cost landscape being searched | **No.** Neither Hamiltonian simulation nor annealing involves a hypothesis space |

## The concrete contrast that makes this unmissable

**"Factor this 2048-bit number":** there's no $$H$$ at all — no candidate functions, no data, nothing to generalize to. It's a single fixed task. What it *does* have is Type A structure (the hidden period of modular exponentiation). Structure lives entirely in the problem, zero involvement from $$H$$.

**"Classify these cat and dog photos":** there absolutely is an $$H$$ — the space of candidate classifiers. Does it have Type A structure? No — there's no group, no hidden subgroup, nothing for a QFT to lock onto. Does it have inductive bias? Yes, obviously — translation symmetry in images, which is why CNNs exist. So this problem has $$H$$ *and* inductive bias, but zero Type A or Type B structure.

These two examples sit at exact opposite ends of the "$$H$$ or no $$H$$" axis — which is precisely why conflating "does this have quantum-exploitable structure" with "what does its hypothesis space look like" leads to confusion. They're independent questions about independent objects.

## How to actually know — the recipe, tied to "what object am I even looking at"

1. **First ask: is there an $$H$$ here at all?** Is this a task where you're fitting a model to finite data and need it to generalize? If no — skip inductive bias entirely, it's not a candidate. Go straight to checking Type A / Type B / outside-the-table against the problem's own definition.

2. **If there's no $$H$$:** look at how the problem itself is stated.
   - Phrased as oracle-query access to a function over a nameable group? → check **Type A**.
   - Phrased as sampling from a physical process's natural output distribution? → check **Type B**.
   - "Make this quantum system's dynamics" or "search a cost landscape"? → **outside the table**.

3. **If there is an $$H$$:** the quantum-advantage question splits into two *separate* sub-questions that shouldn't get merged:
   - (a) Does the learning task itself have inductive-bias structure worth exploiting — a classical architecture-design question, nothing to do with quantum.
   - (b) Completely independently, is there *also* some Type A or Type B structure buried in how you're computing something *within* that pipeline (e.g., a quantum kernel's similarity computation)? That second question gets asked about the specific quantum subroutine, not about $$H$$ itself.

## The cleanest one-line test

**Type A and Type B are properties of a fixed mathematical object** (a function+group, or a physical sampling process) — facts about the problem that don't change no matter who's trying to solve it or how much data anyone has.

**Inductive bias is a property of a learning setup**, and only exists once data and generalization enter the picture at all.

If you're ever unsure which one you're even asking about, that fork — *is there data being generalized from, or is this one fixed task* — is the question to check first.
